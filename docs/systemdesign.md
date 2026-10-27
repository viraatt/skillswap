# System Design & Data Models

## 1. Schema Definitions (Mongoose)

### User Schema (server/src/models/User.js)
```javascript
{
  firebaseUid: { type: String, required: true, unique: true, index: true },
  handle: { type: String, required: true, unique: true, index: true },
  email: { type: String, required: true },
  credits: { type: Number, default: 2, min: 0 },
  skillsOffered: [{
    name: { type: String, required: true, index: true },
    level: { type: String, enum: ["Beginner", "Intermediate", "Advanced"], default: "Intermediate" }
  }],
  skillsWanted: [{
    name: { type: String, required: true, index: true },
    level: { type: String, enum: ["Beginner", "Intermediate", "Advanced"], default: "Beginner" }
  }],
  rating: { type: Number, default: 0.0 },
  completedSwapsCount: { type: Number, default: 0 },
  rank: { type: String, default: "Newbie" }
}
```

### SwapSession Schema (server/src/models/SwapSession.js)
```javascript
{
  requesterId: { type: mongoose.Schema.Types.ObjectId, ref: "User", required: true, index: true },
  mentorId: { type: mongoose.Schema.Types.ObjectId, ref: "User", required: true, index: true },
  skill: { type: String, required: true },
  startTime: { type: Date, required: true },
  endTime: { type: Date, required: true },
  status: { 
    type: String, 
    enum: ["PENDING", "ACCEPTED", "REJECTED", "COMPLETED", "CANCELLED"], 
    default: "PENDING",
    index: true 
  },
  meetLink: { type: String },
  escrowLocked: { type: Boolean, default: false }
}
```

### Review Schema (server/src/models/Review.js)
```javascript
{
  sessionId: { type: mongoose.Schema.Types.ObjectId, ref: "SwapSession", required: true, unique: true },
  targetUserId: { type: mongoose.Schema.Types.ObjectId, ref: "User", required: true, index: true },
  reviewerId: { type: mongoose.Schema.Types.ObjectId, ref: "User", required: true },
  skillScore: { type: Number, min: 1, max: 5, required: true },
  punctualityScore: { type: Number, min: 1, max: 5, required: true },
  recommended: { type: Boolean, default: true },
  createdAt: { type: Date, default: Date.now }
}
```

---

## 2. Dynamic Rating Aggregation Pipeline

Run post-session to recompute the target user credibility rating and assign ranks:

```javascript
[
  { $match: { targetUserId: new mongoose.Types.ObjectId(targetUserId) } },
  {
    $group: {
      _id: "$targetUserId",
      avgSkill: { $avg: "$skillScore" },
      avgPunctuality: { $avg: "$punctualityScore" },
      totalReviews: { $sum: 1 },
      positiveRecommendations: {
        $sum: { $cond: [{ $eq: ["$recommended", true] }, 1, 0] }
      }
    }
  },
  {
    $project: {
      _id: 1,
      totalReviews: 1,
      finalRating: {
        $round: [
          {
            $add: [
              { $multiply: ["$avgSkill", 0.60] },
              { $multiply: ["$avgPunctuality", 0.25] },
              {
                $multiply: [
                  { $divide: ["$positiveRecommendations", { $max: ["$totalReviews", 1] }] },
                  0.75
                ]
              }
            ]
          },
          2
        ]
      }
    }
  }
]
```

# User Activity Analytics Platform (MongoDB & Python)

## Overview
This project implements a user activity analytics platform using MongoDB and Python to transform raw, high-volume event data into meaningful product insights.  
User interactions are modeled as time-ordered events, and MongoDB aggregation pipelines are used to compute key analytics metrics such as Daily Active Users (DAU), funnel conversion, and event trends.

The project demonstrates how document-oriented databases support flexible, scalable analytics workloads beyond basic CRUD operations.

## Business & Analytics Problem
User activity data is inherently event-driven:
- Events evolve over time with new types and attributes
- Analytics often spans large time ranges
- Product teams need fast answers to behavioral questions

Using rigid relational schemas for this data typically results in:
- Frequent schema changes
- Complex joins
- Inefficient time-based analytics

This project addresses these challenges by modeling user activity as append-only event data optimized for analytics use cases.

## Objectives
- Design an event-based data model using MongoDB documents
- Efficiently store and query large volumes of user activity data
- Implement common product analytics metrics (DAU, funnels)
- Apply MongoDB aggregation pipelines for time-series analysis
- Understand design trade-offs in real analytics systems

## Data Model

**Events Collection**
- All user interactions are stored in a single `events` collection
- Each document represents one user action at a specific timestamp

```json
{
  "ts": "2026-01-26T15:45:12Z",
  "user_id": "u_01234",
  "session_id": "s_abcd1234",
  "event": "page_view",
  "properties": {
    "page": "/product/A1"
  },
  "context": {
    "country": "US",
    "device": "mobile",
    "referrer": "google"
  }
}
```

## Indexing Strategy
Indexes were added to support common analytics queries:

- `ts` -> time-range filtering

- `(user_id, ts)` -> distinct user counts and session analysis

- `(event, ts)` -> funnel and event-frequency queries

These indexes reflect real trade-offs between write cost and read performance in analytics systems.

## Analytics Use Cases Implemented
### Engagement Metrics
- Daily Active Users (DAU)
    - Counts distinct users in a given time window using aggregation pipelines

- DAU by Day
   - Groups events into daily buckets using $dateTrunc to show usage trends over time

### Event & Page Insights
- Top Events
    - Identifies the most frequent user actions

- Top Pages
    - Analyzes page views using nested document fields

### Funnel Analysis
- Basic Funnel
    - `product_view → add_to_cart → purchase`
Tracks how many unique users reach each stage and where drop-offs occur

## Data Generation
The event data was pre-populated to simulate realistic user behavior, allowing the project to focus on analytics design and MongoDB usage rather than data simulation.

- 100,000+ user activity events
- Multiple users and sessions
- Realistic time distribution across days
- Natural drop-off between funnel stages

This approach reflects how analytics platforms typically ingest raw event data in production systems.

## Tools Used
- **Python**
- **MongoDB Atlas**
- **PyMongo**
- **FastAPI**
- **Jupyter Notebook** (for exploration and analysis)

## Skills Highlighted
- Event-driven data modeling
- MongoDB aggregation pipelines
- Index design for analytics workloads
- Time-series and funnel analysis
- Understanding trade-offs between schema flexibility and query complexity

## What I Learned
- Analytics systems are built on raw event streams, not summary tables.
- MongoDB is a strong fit for evolving analytics data.
- Aggregation pipelines can replace many traditional SQL analytics patterns
- Accurate funnel and engagement metrics require careful handling of time and event order

## Future Improvements
- Implement full order-aware funnels using MongoDB aggregation
- Add retention analysis (Day-1 / Day-7 cohorts)
- Precompute daily metrics for faster dashboards
- Build basic visualizations on top of analytics results

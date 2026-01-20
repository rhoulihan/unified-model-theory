# Unified Model Theory: Video Script
## Duration: 7 minutes | Presenter: Rick Houlihan

---

### [0:00–0:30] HOOK

You've spent your career choosing sides. Relational or document. Normalize or denormalize. Consistency or developer velocity.

I spent fifteen years at Amazon and MongoDB convincing people that document databases were the answer. I invented single-table design for DynamoDB. I've seen what happens when you go all-in on denormalization at scale—the wins *and* the casualties.

Today I'm going to tell you why that entire debate was the wrong conversation. And why Unified Model Theory changes everything.

---

### [0:30–1:30] THE PROBLEM: TWO INCOMPLETE SOLUTIONS

Let's be honest about what we've been doing for forty years.

Relational databases got the *theory* right. Third normal form. Referential integrity. ACID transactions. The relational model is mathematically sound—it eliminates update anomalies, it guarantees consistency, it gives you a single source of truth.

But it got the *developer experience* wrong. Object-relational impedance mismatch. N+1 query problems. ORMs that generate SQL your DBA wants to strangle you for. Every application ends up building its own aggregation layer just to get data into a usable shape.

Document databases flipped it. They got the developer experience right—your code thinks in objects, your database stores objects, no translation layer. But they got the theory wrong. Denormalization means data duplication. Data duplication means update anomalies. And here's what nobody talks about: *analytics on denormalized data is painful*. You end up ETL-ing back to a relational warehouse anyway.

The industry created an entirely new genre of data storage to solve this. NoSQL promised developer freedom—but traded one set of problems for another.

**This was both a technology problem and a modeling problem. We've been solving only half the equation.**

---

### [1:30–2:30] THE HYBRID PROBLEM: WHERE EVERYTHING BREAKS

Here's where it gets worse. Most real applications aren't purely OLTP or purely OLAP. They're hybrid.

Think about an e-commerce platform. You need low-latency order lookups—that's OLTP, document territory. But you also need real-time revenue dashboards—that's OLAP, relational territory. And then someone asks: "Show me all orders over $500 from California customers in the last hour." That's an ad-hoc analytical query on operational data.

With a document database, that last query is painful or impossible. Your data is denormalized—optimized for reading orders, not for joining customers to orders to filter by state.

So what do teams do? They build CDC pipelines to replicate data to an analytics database. They cache query results and accept stale data. They build aggregation microservices to work around the document model's limitations.

Two databases. Sync lag. Eventual consistency. Cached results. All because no single system could serve all three access dimensions.

---

### [2:30–3:45] UNIFIED MODEL THEORY: THE FRAMEWORK

UMT is built on five concepts. Learn these, and you can evaluate any data architecture.

**Canonical Form.** This is your normalized relational structure. Entities in their own tables. Foreign keys enforcing relationships. Constraints guaranteeing data quality. This is your single source of truth—optimized for integrity, storage efficiency, and analytics.

**Projected Shape.** This is a document projection derived from the canonical form. It's denormalized, self-contained, optimized for a specific consumer. An order service sees orders with embedded customer and line items. A shipping service sees shipments with embedded addresses. Different shapes, same underlying truth.

**Shape Projection.** The declarative mapping between canonical and projected. This is where you define *how* relational tables compose into document structures. It's not ETL. It's not a materialized view you have to refresh. It's a live, bidirectional transformation.

**Access Surface.** The interface through which applications consume data—SQL for analytics, SODA for REST, MongoDB API for compatibility, Oracle Document API for native operations. Same data, multiple interfaces.

**Access Dimension.** This is the new one. Your workload orientation: *Relational* for OLAP analytics, *Document* for OLTP operations, or *Hybrid* for what I call OATP—Operational Analytics and Transaction Processing. Traditional architectures force you to pick one and build workarounds for the others. UMT serves all three from a single source.

The key insight: *relational and document aren't competing models. They're complementary projections of the same data serving different access dimensions.*

---

### [3:45–5:00] IMPLEMENTATION: JSON RELATIONAL DUALITY

Theory's great. Implementation matters. Oracle's JSON Relational Duality is the first implementation of UMT in a production database.

Here's what it actually does.

You define a duality view with a declarative SQL statement. You're composing relational tables into a JSON document shape—nested objects, embedded arrays, exactly the structure your application expects.

```sql
CREATE JSON RELATIONAL DUALITY VIEW order_v AS
SELECT JSON {
  '_id': o.order_id,
  'customer': {'name': c.name, 'email': c.email},
  'items': [SELECT {'sku': li.sku, 'qty': li.quantity}
            FROM line_items li WHERE li.order_id = o.id]
}
FROM orders o
JOIN customers c ON o.customer_id = c.id;
```

Now here's the magic: this isn't a read-only view. When you POST a JSON document to this view, the database *decomposes* it back into normalized inserts and updates. Customer changed their email? The customers table gets updated. Add a line item? It goes into line_items. Full ACID transaction wrapping the whole operation.

No application code. No ORM. No sync jobs. The document *is* the relational data, accessed through a different lens.

And here's what makes it serve all three access dimensions: those same normalized tables are fully queryable via SQL. Your OLTP application reads and writes documents. Your analyst runs SQL joins. Your dashboard queries aggregate in real-time. Same tables. Same transaction. Same consistency.

---

### [5:00–5:45] WHY THIS MATTERS: THE INFRASTRUCTURE YOU DELETE

Let me tell you what disappears when you adopt UMT.

**ORM complexity.** Your application reads and writes documents. The database handles the mapping.

**CDC pipelines.** You don't need to replicate relational data to a document store. There's no replica to sync.

**Analytics ETL.** Your OLTP documents and OLAP queries share the same tables. No warehouse sync required.

**Cached result sets.** You don't need to cache stale analytics results. Query the live data directly.

**Saga patterns.** Document operations are ACID transactions. Distributed coordination becomes local coordination.

**Consistency windows.** There's no "eventually" in this model. Read your write, every time.

**Impedance mismatch.** Developers get documents. Analysts get SQL. DBAs get relations. Same database, same data, different access surfaces serving different access dimensions.

This isn't incremental improvement. It's architectural simplification. You're removing entire categories of infrastructure.

---

### [5:45–6:15] THE THREE ACCESS DIMENSIONS IN PRACTICE

Let me make this concrete.

**Relational dimension—OLAP.** Your analyst asks: "What's our revenue by product category this quarter?" They run a SQL query with joins, aggregations, and filters. Standard OLAP. Works perfectly on the canonical tables.

**Document dimension—OLTP.** Your mobile app needs an order with customer info and line items. It fetches a single JSON document from the duality view. Pre-joined, ready to render. Standard OLTP.

**Hybrid dimension—OATP.** Your ops dashboard needs real-time metrics: orders per minute, average order value, top products right now. That's analytical queries on operational data—Operational Analytics and Transaction Processing. With UMT, you run SQL on the same tables the documents project from. No CDC lag. No stale cache. Real-time OATP.

One database. All three dimensions. Zero compromise.

---

### [6:15–7:00] CLOSE: THE NEW QUESTION

For forty years, architects have asked: "Should we use relational or document?"

Unified Model Theory gives you better questions:

"Where's my canonical form?"
"What shapes do my consumers need?"
"What access dimensions does my workload require?"

Model once. Project for every consumer. Serve every dimension.

That's the future of data architecture. And it's available today in Oracle Database 23ai.

I'm Rick Houlihan. Let's go build something.

---

## PRODUCTION NOTES

**Visuals to prepare:**
- Slide: Relational vs Document tradeoff matrix (both incomplete)
- Slide: Three Access Dimensions diagram (OLAP, OLTP, OATP)
- Animation: Hybrid problem—two databases, CDC arrows, cache layers, "eventual consistency" warning
- Animation: Canonical form projecting into multiple document shapes (prism metaphor)
- Slide: Five UMT concepts with icons (including Access Dimension)
- Animation: Multiple access surfaces and dimensions consuming same canonical form
- Screen capture or animation: JSON Duality View SQL with arrows showing decomposition
- Slide: "Infrastructure you delete" checklist with strikethroughs
- Slide: Three dimensions in practice—same tables serving OLAP, OLTP, and OATP queries

**B-roll opportunities:**
- Code scrolling (SQL, JSON)
- Architecture diagrams simplifying (before/after)
- Dashboard showing real-time metrics

**Tone:** Confident, direct, slightly provocative. You're not selling—you're explaining something you've figured out that changes the game.

**Pacing targets:**
- 0:00–1:30: Build tension (the problem)
- 1:30–2:30: Escalate (the hybrid problem)
- 2:30–3:45: Deliver insight (the theory with five concepts)
- 3:45–5:45: Prove it works (implementation + benefits)
- 5:45–6:15: Make it concrete (three dimensions in practice)
- 6:15–7:00: Land the plane (call to action)

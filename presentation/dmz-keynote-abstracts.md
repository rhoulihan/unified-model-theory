# Data Modeling Zone Keynote - Redwood City, March 2025

## Unified Model Theory: Stop Choosing Between Data Models—Project All of Them

---

## Short-Form Abstract

For 40 years, architects have asked the wrong question: "Should we use relational or document?" The answer was always "yes."

Documents, graphs, time series, vectors, and relations aren't competing paradigms—they're orthogonal projections of the same underlying truth. Unified Model Theory gives you a better question: *Where's my canonical form, and what shapes do my consumers need?*

The polyglot persistence era solved immediate problems while creating architectural debt. Five databases means five consistency models, five sync pipelines, five failure modes—and AI applications that hallucinate because their context arrived stale.

This session cuts through the noise. You'll learn why one canonical form with declarative projections eliminates the integration tax, why your AI strategy is only as good as your data architecture, and why the best code is the code you don't write.

No theory without implementation. Real queries. Real benchmarks. Real infrastructure you can delete.

---

**About the Speaker:**
Rick Houlihan is Field CTO at Oracle with 30+ years in enterprise data architecture. He pioneered DynamoDB single-table design at AWS, led MongoDB's strategic DevRel team, and holds 9 patents. He left MongoDB for Oracle because they built a more elegant solution.

---

## Long-Form Abstract

**Unified Model Theory: The End of Polyglot Persistence and the Architecture AI Actually Needs**

The database wars are over. Not because someone won—because the question was wrong.

For four decades, architects have debated relational versus document, SQL versus NoSQL, normalized versus denormalized. We built careers on these false dichotomies. We shipped MongoDB for APIs, Neo4j for recommendations, InfluxDB for IoT, Pinecone for embeddings, and PostgreSQL for analytics—then spent years building CDC pipelines to keep them synchronized.

The result? Five databases. Five consistency models. Five APIs to learn. Five network hops before your AI sees any context. And when that context arrives milliseconds stale, your RAG pipeline doesn't fail gracefully—it hallucinates with authority.

**Unified Model Theory ends this.**

UMT recognizes that documents, graphs, time series, vectors, and relations are not competing data models. They are orthogonal projections of the same underlying truth. You don't choose between them—you define your canonical form once, then project it into whatever shapes your consumers need. Real-time. Bidirectional. ACID-guaranteed.

This isn't incremental improvement. It's architectural elimination:

- **Documents?** JSON Tables and Duality Views with O(1) field access and MongoDB API.
- **Graphs?** SQL Property Graphs on the same tables—no separate database, no sync.
- **Time Series?** Temporal queries on operational data—no InfluxDB, no replication lag.
- **Vectors?** Native vector indexes with in-database embeddings—no Pinecone, no stale context.
- **Relational?** Still there. Your canonical form. Full SQL, even for MongoDB users with $sql.
- **CDC pipelines?** Deleted. The projection *is* the data.
- **Eventual consistency?** Gone. One query, one transaction, zero lag.

In this session, you'll see what happens when you stop treating data models as infrastructure decisions and start treating them as access patterns. You'll learn to ask the questions that actually matter: Where's my canonical form? What projected shapes do my consumers need? What access dimensions does my workload require? Can my AI afford eventual consistency?

You'll see real implementations—not slideware. Single SQL queries that combine vector similarity search, document retrieval, graph traversal, and time series aggregation in one transaction. Real benchmarks that quantify the integration tax you've been paying. Real infrastructure you can delete from your architecture diagrams.

The polyglot persistence era taught us valuable lessons about developer experience and specialized workloads. Unified Model Theory takes those lessons and eliminates the architectural debt. One database. One consistency model. One optimizer. Every shape your applications need.

Your AI strategy is only as good as your data architecture. If your RAG pipeline crosses four network boundaries, you're not building AI—you're building latency. It's time for a better architecture.

---

**About the Speaker:**

Rick Houlihan is Field CTO for JSON Duality at Oracle, where he focuses on developer adoption of converged database architecture. Over 30 years in enterprise data, Rick has led transformative initiatives at IBM, AWS, and MongoDB—pioneering DynamoDB single-table design, facilitating 4,000+ RDBMS-to-NoSQL migrations, and building MongoDB's industry-leading DevRel revenue program. He holds 9 patents spanning Complex Event Processing, Microprocessor Design, NoSQL, and VM Hypervisor technology. His AWS re:Invent sessions on advanced data modeling were the most-viewed talks for three consecutive years. Rick joined Oracle because after spending 15 years advocating for document databases, he found something better: a unified architecture that makes the relational-versus-document debate obsolete.

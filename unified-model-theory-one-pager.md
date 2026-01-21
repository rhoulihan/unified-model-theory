# Unified Model Theory
## One Canonical Form, Every Projected Shape

---

**The False Dichotomy**

For decades, architects faced impossible choices: relational integrity or document agility? Graph relationships or time series analytics? Every data model meant another database, another sync pipeline, another consistency boundary.

The industry built polyglot persistence—MongoDB for documents, Neo4j for graphs, InfluxDB for time series, PostgreSQL for relations. Five databases, five consistency models, five teams maintaining sync pipelines.

This was never a technology problem. It was a modeling problem—and we've been solving the wrong equation.

---

**A New Framework**

**Unified Model Theory (UMT)** recognizes that documents, graphs, time series, and relations are not competing paradigms. They are *orthogonal projections of the same underlying truth*.

| Concept | Definition |
|---------|------------|
| **Canonical Form** | The normalized relational structure—your single source of truth |
| **Projected Shape** | Any data model (document, graph, time series, vector) optimized for a specific access pattern |
| **Shape Projection** | The declarative mapping from canonical form to any consumable shape |
| **Access Surface** | The interface layer—SQL, document API, graph queries, or all three |

Under UMT, you don't choose between data models. You model your domain canonically, then *project* it into whatever shapes your consumers require.

---

**Every Data Model is a Projection**

| Projected Shape | Optimized For | Same Canonical Data |
|-----------------|---------------|---------------------|
| **Document (JSON)** | API responses, pre-joined reads | ✓ |
| **Graph** | Relationship traversal, recommendations | ✓ |
| **Time Series** | Temporal queries, trend analysis | ✓ |
| **Relational** | Ad-hoc analytics, complex joins | ✓ |
| **Vector** | Similarity search, AI/RAG | ✓ |

**All shapes. One source of truth. Zero sync.**

---

**The Core Principle**

> **Model once. Project as documents, graphs, time series, or relations. Serve every consumer from one source of truth.**

One truth. Every shape. Zero tradeoffs.

---

**From Theory to Implementation**

Oracle Database 26ai implements UMT with native support for every projected shape:

```sql
-- Document projection
CREATE JSON RELATIONAL DUALITY VIEW order_doc AS ...

-- Graph projection (same tables)
CREATE PROPERTY GRAPH ecommerce_graph ...

-- Time series query (same tables)
SELECT time_bucket('1 hour', order_date), SUM(total) FROM orders ...

-- Combined AI query (vectors + docs + graph + time series)
SELECT JSON {
  'context': v.chunk_text,
  'related': (SELECT ... FROM GRAPH_TABLE(...)),
  'trends': (SELECT ... WHERE order_date > SYSTIMESTAMP - 30)
}
FROM vectors v WHERE VECTOR_DISTANCE(...) < 0.3;
```

**One insert updates every projection. No sync. No CDC. No lag.**

---

**Why This Matters Now: AI Applications**

AI applications need vectors + documents + graph + time series in the same query.

| Polyglot Stack | UMT (Oracle) |
|----------------|--------------|
| 5 databases, 5 sync pipelines | 1 database, 0 sync |
| 5 consistency models | 1 consistency model |
| Context may be stale | Context always consistent |
| Hallucinations likely | Hallucinations prevented |

---

**Infrastructure You Delete**

| Traditional | With UMT |
|-------------|----------|
| MongoDB | JSON Duality Views |
| Neo4j | SQL Property Graph |
| InfluxDB | Temporal queries |
| Pinecone | Native vectors |
| CDC pipelines | Eliminated |
| ETL jobs | Eliminated |

---

**The Path Forward**

1. **Where is your canonical form?** Every copy is a liability.
2. **What shapes do you need?** Documents? Graphs? Time series? All three?
3. **What's your projection latency?** Synchronous (UMT) vs. eventual (CDC).
4. **What does your AI require?** Eventual consistency = hallucination risk.

---

> *Model once. Project as documents, graphs, time series, or relations.*
> *Serve every consumer from one source of truth.*

**Oracle Database 26ai** — Unified Model Theory, implemented.

---

*For technical deep-dives, hands-on workshops, and architectural guidance, contact the Oracle Developer Enablement team.*

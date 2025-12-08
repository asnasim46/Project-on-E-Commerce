**ERD (Mermaid) for Project-on-E-Commerce**

Files added:

- `design/ERD.mmd` — Mermaid `erDiagram` describing tables and primary relationships.

How to view
- In VS Code: open `design/ERD.mmd` inside a Markdown preview that supports Mermaid (install "Markdown Preview Mermaid Support" or use the built-in Mermaid preview if available).
- You can also copy the `erDiagram` block into any Mermaid live editor (https://mermaid.live) to render and export SVG/PNG.

Notes & assumptions
- I used the table/column list you provided and inferred foreign-key relationships from columns that end with `_FK` or matching names (e.g., `customer_id_FK` -> `CUSTOMERS.customer_id`).
- Data types are generic (int, string, numeric, datetime) — please confirm the target RDBMS (Oracle/Postgres/MySQL) and I'll generate dialect-specific DDL.
- Some columns in your list had typos / inconsistent names (e.g., `adress`, `contanct_person_designation`, `Warrenty`) — I normalized names in the ERD to be readable; confirm preferred spellings.

Next steps I can take for you
- Generate SQL DDL for a chosen target (Oracle/Postgres/MySQL). Tell me which dialect to target.
- Produce a more detailed ERD (showing nullability, indexes, and cardinalities) if you provide additional constraints.
- Export a high-resolution SVG/PNG of the diagram (I can create the SVG by converting Mermaid to SVG here if you prefer).

If you want changes to table names, field names, or relationships, tell me the exact edits and I will update `design/ERD.mmd`.

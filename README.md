# E-Commerce Inventory Management Backend Task

## Task Overview
ShopFlow is an e-commerce platform managing products across multiple suppliers, categories, and warehouses. Your task is to design the PostgreSQL schema and implement database logic for a FastAPI application that handles complex inventory workflows, such as stock tracking per warehouse, price changes with historical tracking, low-stock detection, movement logging, and purchase order processing. Implementing both the normalized schema and async FastAPI integration is essential to ensure ShopFlow operates reliably, scales for high concurrency, and maintains correct real-time inventory data for business continuity and growth.

## Guidance
- Check the `/root/task/app/` directory for the pre-built FastAPI scaffold (including routers and schemas) and the `/root/task/app/routes/api.py` file for incomplete endpoint logic.
- Both `/root/task/schema.sql` and `/root/task/data/sample_data.sql` are empty: design your own schema, including advanced relationships, multi-level category hierarchies, indexes, constraints, and at least one materialized view for reporting.
- All DB access in FastAPI routes should be written using **async-compatible raw SQL** patterns (no ORM) and non-blocking wrappers. Review provided connection helpers.
- Implement complex logic: paginated async product search with category filter, atomic inventory updates with async movement logging (background task), low-stock detection with reorder suggestion, bulk price updates, warehouse transfer validation, and inventory value reporting.
- Provide transactional integrity (ACID) in async code and handle errors with custom responses.
- Pay attention to avoiding N+1 queries, and create indexes or partial indexes where needed for performance.

## Database Access
- **Host:** `<DROPLET_IP>`
- **Port:** 5432
- **Database:** shopflow
- **User:** shopuser
- **Password:** shoppass

You may connect directly with psql/DBeaver/pgAdmin for DB access. Test implemented endpoints via Postman or curl as you build out async logic.

## Objectives
- Model entities: products, multi-level categories, suppliers, warehouses, inventory per warehouse, price histories, inventory movements, purchase orders, and order items with proper normalization and foreign key relationships.
- Implement check constraints for valid status fields, positive quantities/prices, and data integrity.
- Efficiently seed hierarchical and realistic sample data as per requirements.
- Complete all API logic using async-compatible DB operations, ensuring endpoints are non-blocking and performant.
- Integrate inventory movement logging as an async background task where possible.
- Implement a materialized view/reporting function for warehouse-wise inventory valuation.
- Enforce transactional safety and advanced error handling in all relevant endpoints.

## How to Verify
- Confirm schema, indexes, and relationships by inspecting the DB with a SQL client after seeding sample data.
- Use API tools to:
    - Search products (filter/paginate); verify async responses obey category filtering and pagination.
    - Update inventory: ensure stock/warehouse updates reflect correctly and all movements are logged (and not blocking main thread).
    - Receive low-stock alerts and reorder suggestions for SKUs with low quantity.
    - Perform bulk price updates and verify historical price tracking.
    - Execute warehouse transfers; validate inventory adjustments and rules.
    - Query/report inventory value per warehouse using your implemented view or logic.
- Check that errors (e.g., negative quantities, invalid categories, out-of-stock, transfer to disabled warehouse) return meaningful responses.
- Ensure all DB interactions in endpoints are non-blocking and perform well for small concurrency stress tests.

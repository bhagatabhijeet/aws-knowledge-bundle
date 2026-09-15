# DynamoDB Folder Update Log

## 2026-09-15
* **Creation**: Authored the full DynamoDB concept set (what-is-dynamodb, tables-items-and-attributes, primary-keys-and-partitions, querying-vs-scanning, secondary-indexes, capacity-modes, streams-ttl-and-global-tables, best-practices, mnemonics-cheatsheet, glossary) plus original SVG diagrams under `assets/images/`, all built on the "Valet Parking Garage" mnemonic analogy.
* **Creation**: Added [hands-on-customer-orders-table.md](hands-on-customer-orders-table.md), a practical tutorial using an e-commerce "customer order history" use case — creating a real table (`CustomerOrders`, Partition Key `CustomerId` + Sort Key `OrderId`), inserting schemaless items, and demonstrating GetItem, Query (including a Sort Key range condition), and Scan, with both AWS Console steps and copy-pasteable AWS CLI commands.

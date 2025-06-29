# Neo4j GraphDB Docker Template

Neo4j GraphDB is a native graph database that stores data in nodes and relationships, making it perfect for complex queries and relationship-heavy data. It's widely used for social networks, recommendation systems, fraud detection, and knowledge graphs.

## About This Template

This Docker template provides a pre-configured Neo4j instance with bulk data import capabilities, configurable memory settings, and easy deployment. It includes automatic data import from CSV files and customizable memory allocation for optimal performance.

## Common Use Cases

- Social network analysis and friend recommendations
- Fraud detection and pattern recognition
- Knowledge graphs and semantic search
- Supply chain optimization and logistics
- Real-time recommendation engines

## Features

- **Bulk Data Import**: Import nodes and relationships from CSV files via URLs
- **Configurable Memory**: Customize heap and page cache memory settings
- **Pre-configured Logging**: User and server logs enabled by default
- **Docker Ready**: Easy deployment with Docker

## Dependencies

- Docker
- Neo4j 5.25.1
- Optional: CSV files for data import

## Quick Start

### Basic Deployment

```bash
docker build --build-arg DB_PASSWORD=your-password -t my-neo4j .
docker run -p 7474:7474 -p 7687:7687 my-neo4j
```

### With Custom Memory Settings

```bash
docker build \
  --build-arg HEAP_INITIAL_SIZE=2g \
  --build-arg HEAP_MAX_SIZE=2g \
  --build-arg PAGECACHE_SIZE=8g \
  --build-arg DB_PASSWORD=your-password \
  -t my-neo4j .
```

### With Data Import

```bash
docker build \
  --build-arg NODE_CSV_URLS="https://example.com/nodes.csv" \
  --build-arg RELATION_CSV_URLS="https://example.com/relationships.csv" \
  --build-arg DB_PASSWORD=your-password \
  -t my-neo4j .
```

## Configuration Options

### Build Arguments

| Argument            | Default | Required | Description                                     |
| ------------------- | ------- | -------- | ----------------------------------------------- |
| `DB_PASSWORD`       | `""`    | **Yes**  | Neo4j database password                         |
| `HEAP_INITIAL_SIZE` | `"1g"`  | No       | Initial heap memory size                        |
| `HEAP_MAX_SIZE`     | `"1g"`  | No       | Maximum heap memory size                        |
| `PAGECACHE_SIZE`    | `"4g"`  | No       | Page cache memory size                          |
| `NODE_CSV_URLS`     | `""`    | No       | Comma-separated URLs for node CSV files         |
| `RELATION_CSV_URLS` | `""`    | No       | Comma-separated URLs for relationship CSV files |

### Environment Variables

The same build arguments can also be passed as environment variables when running the container.

## Connection Examples

### JavaScript (Node.js)

```javascript
import neo4j from "neo4j-driver";

const host = "neo4j://localhost:7687";
const username = "neo4j";
const password = "your-password";

const driver = neo4j.driver(host, neo4j.auth.basic(username, password));
const result = await driver.executeQuery("MATCH (n) RETURN n");
```

### Python

```python
from neo4j import GraphDatabase

uri = "neo4j://localhost:7687"
username = "neo4j"
password = "your-password"

driver = GraphDatabase.driver(uri, auth=(username, password))
with driver.session() as session:
    result = session.run("MATCH (n) RETURN n")
```

## Ports

- `7474`: HTTP port (Neo4j Browser)
- `7687`: Bolt port (Database connections)

## Memory Configuration

For production deployments, consider adjusting memory settings based on your data size and available resources:

- **Small datasets**: Use default settings (1g heap, 4g page cache)
- **Medium datasets**: 2g heap, 8g page cache
- **Large datasets**: 4g+ heap, 16g+ page cache

### Memory Settings Reference

For detailed information about Neo4j memory configuration, refer to the official documentation:

- [Heap Initial Size](https://neo4j.com/docs/operations-manual/current/configuration/configuration-settings/#config_server.memory.heap.initial_size)
- [Heap Max Size](https://neo4j.com/docs/operations-manual/current/configuration/configuration-settings/#config_server.memory.heap.max_size)
- [Page Cache Size](https://neo4j.com/docs/operations-manual/current/configuration/configuration-settings/#config_server.memory.pagecache.size)

## License

This template is open source and available under the MIT License.

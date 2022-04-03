# flightly

This repo is a collection of resources from my final project for the `SCS 3252:017 Big Data Management Systems & Tools` course (University of Toronto) taken in the last four months of 2019. It includes the final paper [Comparing Neo4j with PostgreSQL](./Big%20Data%20Course_%20Comparing%20Neo4j%20with%20PostgreSQL.pdf) as well as the accompanying [Jupyter notebook](./Flightly%20Comparing%20Neo4j%20and%20PostgreSQL.ipynb).

### TL;DR

_Neo4j can be substantially faster for certain types of queries because of the benefits gained from index-free adjacency._

**Example: Get the names of destination airports from all flights originating in Wyoming**

Cypher (Neo4j query language):
```
MATCH (hi:Airport {state: 'WY'})-[:HAS_DEPARTURE]->(fl:Flight)-[:FLIES_TO]->(ap:Airport)
```

Postgres SQL:
```
RETURN DISTINCT ap.name
SELECT name from airports
JOIN flights ON (airports.iata = flights.destination_airport)
WHERE flights.origin_airport IN
(SELECT iata from airports WHERE airports.state = 'WY')
```

| Attempt | 1 | 2 | 3 | Average |
| ----------- | ----------- | ----------- | ----------- | ----------- |
| **Neo4j** | 0.29222047s | 0.34756797s | 0.30003158s |  **0.32918392s** |
| **PostgreSQL** | 2.32982001s | 2.14832110s | 2.39080437s | **2.29873976s** |

# ✨ PostgreSQLStorage

**PostgreSQLStorage** is Indexer wrapper around the PostgreSQL DBMS. Postgres is an open source object-relational database. You can read more about it here: https://www.postgresql.org/


**Table of Contents**

- [🌱 Prerequisites](#-prerequisites)
- [🚀 Usages](#-usages)
- [🎉️ Example](#-example)
- [🔍️ Reference](#-reference)


## 🌱 Prerequisites

> These are only needed if you download the source code and directly use the class. Not needed if you use the Jina Hub method below.

- This Executor works on Python 3.7 and 3.8. 
- Make sure to install the [requirements](requirements.txt)

Additionally, you will need a running PostgreSQL database. This can be a local instance, a Docker image, or a virtual machine in the cloud. Make sure you have the credentials and connection parameters. 

You can start one in a Docker container, like so: 

```bash
docker run -e POSTGRES_PASSWORD=123456  -p 127.0.0.1:5432:5432/tcp postgres:13.2 
```

📕 **Note on docker network for macOS users**:  
If you run both the database and the `PostgresSQLStorage` docker container on the same machine 
localhost in the `PostgresSQLStorage` resolves to a separate network created by Docker which cannot see the database running on the host network.  
Use `host.docker.internal` to access localhost on the host machine. You can pass this parameter 
to the `PostgresSQLStorage` storage by using `uses_with={'hostname': 'host.docker.internal''}` when
calling the `flow.add(...)` function.

## 🚀 Usages

This indexer assumes a PRIMARY KEY on the `id` field, thus you cannot add two `Document` of the same id. Make sure you clean up any existing data if you want to start fresh. 

### 🚚 Via JinaHub

#### using docker images
Use the prebuilt images from JinaHub in your Python code: 

```python
from jina import Flow
	
f = Flow().add(uses='jinahub+docker://PostgreSQLStorage')
```

or in the `.yml` config.
	
```yaml
jtype: Flow
pods:
  - name: indexer
    uses: 'jinahub+docker://PostgreSQLStorage'
```

#### using source code
Use the source code from JinaHub in your Python code:

```python
from jina import Flow
	
f = Flow().add(uses='jinahub://PostgreSQLStorage')
```

or in the `.yml` config.

```yaml
jtype: Flow
pods:
  - name: indexer
    uses: 'jinahub://PostgreSQLStorage'
```

## 🎉️ Example 


```python
from jina import Flow, Document

f = Flow().add(uses='jinahub://PostgreSQLStorage')

with f:
    resp = f.post(on='/index', inputs=Document(), return_results=True)
    print(f'{resp}')
```

### Inputs 

Any type of `Document`.

### Returns

Nothing. The `Documents`s are stored.

## 🔍️ Reference

- https://www.postgresql.org/


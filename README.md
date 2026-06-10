# udemy-apache-airflow-the-hands-on-guide
Repository for the exercises of the Udemy Course "Apache Airflow: The Hands-On Guide"

## Pre-requesites
1. Install [Docker Desktop](https://www.docker.com/products/docker-desktop/):
```bash
 sudo apt-get update
 sudo apt install ./docker-desktop-amd6-deb
```
2. Install [Astro CLI](https://www.astronomer.io/docs/astro/cli/install-cli):
```bash
curl -sSL install.astronomer.io | sudo bash -s
```

This project uses Astro CLI version `1.42.1` and Airflow version `2.11.2+astro.3`


## Course

### The basics of Apache Airflow

#### Why data orchestration?


#### Why Airflow?
- Reliably orchestrate data workflows at scale (distributed architecture)
- Integrations (connectors) and customizations
- Easy to use with Python
- Monitoring and Data Lineage (OpenLineage)
- Very active project and widely used.

#### Airflow Core Components
- Web Server: user interfaces
- Scheduler: Schedule tasks while checking dependencies are met
- Meta Database: Contains Airflow metada (tasks instances, workflow, etc.)
- Executor: Defines how and on which system to execute tasks
- Triggerer: Used for a special type of tasks (deferrable operators)
- Worker: Execute the tasks

#### Airflow Core Concepts:
- **Task**: unit of execution build by using operators.
- **DAG** (Directed Acyclic Graph): set of tasks linked by dependencies;

#### How does Airflow work?
1. Create a new data pipeline as a __python file__ in the DAGs directory.
2. The scheduler parse the DAGs directory every five minutes (by default) and serializes it into the Meta Database.
3. The scheduler starts DAGs based on their configurations by creating a Task Instance and sending it to the executor.
4. The executor push the tasks in a queue.
5. Worker processes execute the tasks from the queue.
6. Tasks status are actualized in the Meta Data database.
7. When all tasks of a DAG are finished, the DAG is marked as finished.

### Airflow advanced configuration
#### Concurrency settings:
Requires using an executor other than the __SequentialExecutor__:
- __parallelism:__ the maximum number of tasks that can run concurrently on each scheduler within a single Airflow environment.
- __max_active_tasks_per_dag:__ the maximum number of tasks that can be scheduled at the same time across all runs of a DAG.
  - At the DAG level: __max_active_tasks__.
- __max_active_runs_per_dag:__ determines the maximum number of active DAG runs (per DAG) that the scheduler can create at a time.
  - At the DAG level: __max_active_runs__.
- __max_active_tis_per_dag:__ determines the maximum number of task instance across all DAG runs(at the task level).

#### CeleryExecutor
__Celery__ is a distributed task queue. Allows using multiple _Airflow workers_ in parallel.
__Flower__ tool to monitor celery tasks.
__Workers__ different machines executing tasks. Workers can have different configurations (ex: more or less CPUs/GPUs).
  - __worker_concurrency:__ determines the maximum number of tasks ran at the same time (by default: 16).
  - use different __queues__ to handle the specialized workers.

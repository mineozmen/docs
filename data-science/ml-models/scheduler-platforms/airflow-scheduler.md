---
description: Models can be scheduled on Airflow for automated DAG creation and execution.
---

# Airflow Scheduler

When the scheduler is selected as Airflow, a model has the following additional settings:

* **DAG Name:** Unique name to assign to Airflow DAG (defaults to model-\[id])
* **Deployer:** Deployer module to use for creating model DAG with the following options, which can be extended:
  * **DAG\_Tensor:** Used for TensorFlow models, deployable to k8s or dockers, is default
  * **DAG\_Spark:** Used for PySpark models, deployable to Spark clusters, k8s or dockers
  * **DAG\_Custom:** Used for other models, deployable to k8s or dockers
* **Deployment Mode:** Deployment approach for executing actual model on a remote server
  * **Kubernetes:** With "k8s:image" and "k8s:venv" alternatives, deploying on a k8s cluster using a prebuilt image or venv with related parameters
  * **Docker:** With "docker:image" and "docker:venv" alternatives, deploying on a remote docker using a prebuilt image or venv with related parameters
  * **Spark:** With "spark:submit" and "spark:ssh" alternatives, deploying on a Spark cluster using spark-submit locally or after SSH to a cluster node
* **Deployment Parameters:** Spark, k8s and docker specific parameters to pass on to deployer for execution (e.g. node pool)
* **Model Connections:** List of connections defined in Airflow whose connection descriptors should be passed on to the model executor (to allow central management of connections)
* **Load Model Data:** Whether DAG should load latest model configuration before each run
* **Notify on Update:** Whether DAG should automatically update model version after each run, triggering journals / pulses for other processes (typically used for backend API inference model updates with new trainings)
* **Process Parameters:** Deployment mode specific parameters to pass on to model for execution
* **DAG Args:** Airflow DAG arguments to pass on (e.g. schedule\_interval)

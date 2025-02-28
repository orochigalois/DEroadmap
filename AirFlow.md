# 2025-02-11 13:29:56 What operator you can use to ingest files from GCS to Big Query
reference: xxxxx

Step 1 Detect a new file in GCS bucket (source) and load it into BigQuery
GCSObjectExistenceSensor + GCSToBigQueryOperator

Step 2 Export processed data from BigQuery back to another GCS bucket
BigQueryToGCSOperator

Step 3 Transfer data from GCS bucket to SFTP server for another team
SFTPOperator
_______________________________________________________________
# 2025-02-11 14:25:37 what airflow operator can you use to move data from one bigquery table to another bigquery table? What's the library you can use of?
reference: xxxxx

In Apache Airflow, you can use the **`BigQueryToBigQueryOperator`** to move data from one BigQuery table to another. This operator allows you to copy or append data between tables efficiently.
it's from this libray: airflow.providers.google.cloud.transfers.bigquery_to_bigquery
#### **Example Usage:**

```python
from airflow.providers.google.cloud.transfers.bigquery_to_bigquery import BigQueryToBigQueryOperator

move_bq_table = BigQueryToBigQueryOperator(
    task_id="move_data",
    source_project_dataset_table="source_project.dataset.source_table",
    destination_project_dataset_table="destination_project.dataset.destination_table",
    write_disposition="WRITE_TRUNCATE",  # Options: WRITE_TRUNCATE, WRITE_APPEND, WRITE_EMPTY
    create_disposition="CREATE_IF_NEEDED",
    dag=dag,
)

```

#### **Key Parameters:**

-   `source_project_dataset_table`: The source table in **`project.dataset.table`** format.
-   `destination_project_dataset_table`: The target table in **`project.dataset.table`** format.
-   `write_disposition`: Defines how data is written:
    -   `"WRITE_TRUNCATE"`: Overwrites the destination table.
    -   `"WRITE_APPEND"`: Appends to the destination table.
    -   `"WRITE_EMPTY"`: Fails if the destination table is not empty.
-   `create_disposition`: `"CREATE_IF_NEEDED"` ensures the destination table is created if it doesn't exist.
_______________________________________________________________
# 2025-02-13 17:13:02 in airflow, team A has a DAG_A, I'm in team B, and I need to write a DAG which only when DAG_A is successfully executed, my DAG_B can be triggered, what can I design in this case?
reference: xxxxx

### **Solution 1: Using `ExternalTaskSensor` (Recommended)**

This sensor waits for a specific task in **DAG\_A** to complete before proceeding with **DAG\_B**.

```python

`from airflow.sensors.external_task import ExternalTaskSensor
from airflow import DAG
from airflow.operators.dummy import DummyOperator
from datetime import datetime, timedelta

default_args = {
    'owner': 'team_B',
    'depends_on_past': False,
    'start_date': datetime(2024, 2, 13),
    'retries': 1,
    'retry_delay': timedelta(minutes=5),
}

dag_b = DAG(
    'DAG_B',
    default_args=default_args,
    schedule_interval=None,  # Set it to None if triggered by DAG_A
    catchup=False,
)

wait_for_dag_a = ExternalTaskSensor(
    task_id='wait_for_dag_a',
    external_dag_id='DAG_A',  # The DAG_A to wait for
    external_task_id=None,  # Wait for the entire DAG_A to finish
    mode='poke',  # You can also use 'reschedule'
    timeout=3600,  # Adjust timeout as needed
    allowed_states=['success'],
    failed_states=['failed', 'skipped'],
    dag=dag_b,
)

start_task = DummyOperator(task_id='start_task', dag=dag_b)

wait_for_dag_a >> start_task
`

```

-   This ensures **DAG\_B** only runs after **DAG\_A** has successfully completed.
-   You can customize it to wait for a specific task in **DAG\_A** instead of the whole DAG.



### **Solution 2: Using `TriggerDagRunOperator`**

If **Team A** can modify **DAG\_A**, they can use `TriggerDagRunOperator` to start **DAG\_B** after **DAG\_A** completes.

In **DAG\_A**:

```python


`from airflow.operators.trigger_dagrun import TriggerDagRunOperator

trigger_dag_b = TriggerDagRunOperator(
    task_id="trigger_dag_b",
    trigger_dag_id="DAG_B",
    wait_for_completion=False,  # Set to True if you need to wait for DAG_B to finish
    dag=dag_a,
)
`

```

This will automatically trigger **DAG\_B** once **DAG\_A** completes.


### **Choosing Between the Two**

-   **Use `ExternalTaskSensor`** if **Team A cannot modify DAG\_A**.
-   **Use `TriggerDagRunOperator`** if **Team A agrees to trigger DAG\_B**.

### **Solution 3: trigger file**

Once your data ready, send the trigger file to GCS buckets or something and then the downstream can check this touch file to know it's ready to consume

### **Solution 4: use a central table**

use a central table to store all the status of table update. Kind of like a log table. If the table refresh, we write one more entry over there. Tell people that this table is ready to load
_______________________________________________________________
# 2025-02-28 20:39:54 explain the following code
keywords:
schedule
KubernetesPodOperator
arguments

reference: xxxxx

```python
"""
This DAG is to execute the DBT bluefield supers model, it is deployed via the
KubernetesPodOperator into the Composer Cluster for execution.
When in the default namespace, it will pick up the default project compute
service account for authentication and run in the main node pool.
"""

from airflow import models
from datetime import datetime, timedelta
from airflow.providers.cncf.kubernetes.operators.kubernetes_pod import KubernetesPodOperator
from airflow.kubernetes.secret import Secret
from kubernetes.client import models as k8s

START_DATE=datetime(2023,1,18)

default_args = {
    'email':models.Variable.get('email_failure_notifications'),
    'email_on_failure':True,
    'email_on_retry':False
}


with models.DAG(
    'dbt_bluefield_supers',
    schedule="0 23 * * 6", # 11pm UTC ==> 9am AEST (Sunday)
    start_date=START_DATE,
    catchup=False,
    max_active_runs=1,
    default_args=default_args,
    params={"is_run_by_au_campaign_id":False,
            "au_capaign_ids":""},
) as dag:

    dbt_bluefield_supers_run = KubernetesPodOperator(
        task_id='dbt_bluefield_supers',
        name='dbt_bluefield_supers',
        image='gcr.io/gcp-wow-cart-data-prod-d710/dbt_cartology_cdip:prod',
        namespace='default',
        service_account_name='dbt',
        # When targeting environment tags we should always refresh our image
        image_pull_policy='Always',
        # entrypoint is dbt, cmds=[] to overwrite
        arguments=['run', '-t', 'prod', '--select', 'tag:au_campaign tag:au_campaign_media'],
        env_vars=[
            k8s.V1EnvVar('DBT_VAR_LAST_RUN_TIMESTAMP', '{{ prev_start_date_success }}'),
            k8s.V1EnvVar('AEAD_PROJECT', 'gcp-wow-cart-aead-prod-225d'),
            k8s.V1EnvVar('IS_RUN_BY_AU_CAMPAIGN_ID', '{{ params.is_run_by_au_campaign_id }}'),
            k8s.V1EnvVar('AU_CAMPAIGN_IDS', '{{ params.au_capaign_ids }}'),
            k8s.V1EnvVar('DBT_AUTH_METHOD', 'oauth'),
        ],
        container_resources=k8s.V1ResourceRequirements(
            requests={'cpu': '150m', 'memory': '200M'},
            limits={'cpu': '300m', 'memory': '400M'}
        ),
        # Specifies path to kubernetes config. If no config is specified will default to ~/.kube/config the config_file is templated
        config_file='/home/airflow/composer_kube_config',
        # Identifier of connection that should be used
        kubernetes_conn_id='kubernetes_default',
    )

    dbt_bluefield_supers_test = KubernetesPodOperator(
        task_id='dbt_bluefield_supers_test',
        name='dbt_bluefield_supers_test',
        image='gcr.io/gcp-wow-cart-data-prod-d710/dbt_cartology_cdip:prod',
        namespace='default',
        service_account_name='dbt',
        # When targeting environment tags we should always refresh our image
        image_pull_policy='Always',
        # entrypoint is dbt, cmds=[] to overwrite
        arguments=['test', '-t', 'prod', '--select', 'tag:au_campaign tag:au_campaign_media'],
        env_vars=[
            k8s.V1EnvVar('DBT_VAR_LAST_RUN_TIMESTAMP', '{{ prev_start_date_success }}'),
            k8s.V1EnvVar('AEAD_PROJECT', 'gcp-wow-cart-aead-prod-225d'),
            k8s.V1EnvVar('IS_RUN_BY_AU_CAMPAIGN_ID', '{{ params.is_run_by_au_campaign_id }}'),
            k8s.V1EnvVar('AU_CAMPAIGN_IDS', '{{ params.au_capaign_ids }}'),
            k8s.V1EnvVar('DBT_AUTH_METHOD', 'oauth'),
        ],
        container_resources=k8s.V1ResourceRequirements(
            requests={'cpu': '150m', 'memory': '200M'},
            limits={'cpu': '300m', 'memory': '400M'}
        ),
        # Specifies path to kubernetes config. If no config is specified will default to ~/.kube/config the config_file is templated
        config_file='/home/airflow/composer_kube_config',
        # Identifier of connection that should be used
        kubernetes_conn_id='kubernetes_default',
    )

    # Ensures that run is completed before running tests
    # Tests in this scenario will become non-blocking i.e. raise Errors or Warnings but models will complete running
    dbt_bluefield_supers_run >> dbt_bluefield_supers_test
```
_______________________________________________________________
## Google Africa Developer Scholarship 2020 - Cloud Track

### Introduction

Lab - GCP Fundamentals Getting started with BigQuery. In this lab i explored creating and querying a dataset with BigQuery using Cloud Shell and Cloud SDK.

### Steps

*  Start a cloud shell session and create a BigQuery dataset logdata in Location US

    *  
    ```console
       student_03_0c36349759d9@cloudshell:~ (qwiklabs-gcp-03-e852ea611147)$ bq --location=US mk -d logdata
       Dataset 'qwiklabs-gcp-03-e852ea611147:logdata' successfully created.
    ```   
       
*  Show created dataset

    * 
    ```console
        student_03_0c36349759d9@cloudshell:~ (qwiklabs-gcp-03-e852ea611147)$ bq ls
        datasetId
        -----------
        logdata
    ```
       
*  Load data into table accesslog within the dataset logdata from a Cloud Storage bucket

    *  
    ```console
        student_03_0c36349759d9@cloudshell:~ (qwiklabs-gcp-03-e852ea611147)$ bq load --autodetect logdata.accesslog gs://cloud-training/gcpfci/access_log.csv
        Waiting on bqjob_r1cbb8bb87bf0575f_00000174760cc8f5_1 ... (24s) Current status: DONE
    ```

*  Preview the schema and other properties of the table that has been created

    *
    ```console
       student_03_0c36349759d9@cloudshell:~ (qwiklabs-gcp-03-e852ea611147)$ bq show logdata.accesslog                                                                                                    
       Table qwiklabs-gcp-03-e852ea611147:logdata.accesslog
        Last modified              Schema             Total Rows   Total Bytes   Expiration   Time Partitioning   Clustered Fields   Labels
       ----------------- ---------------------------- ------------ ------------- ------------ ------------------- ------------------ --------
       10 Sep 06:26:43   |- string_field_0: string    1461378      319357700
                    |- string_field_1: string
                    |- string_field_2: string
                    |- int64_field_3: integer
                    |- string_field_4: string
                    |- int64_field_5: integer
                    |- int64_field_6: integer
                    |- int64_field_7: integer
                    |- int64_field_8: integer
                    |- string_field_9: string
                    |- string_field_10: string
                    |- int64_field_11: integer
                    |- int64_field_12: integer
                    |- string_field_13: string
                    |- string_field_14: string

    ```                

*   Run a query on the table using the bq command

    *
    ```console
       student_03_0c36349759d9@cloudshell:~ (qwiklabs-gcp-03-e852ea611147)$ bq query "select int64_field_6 as hour,count(*) as hitcount from logdata.accesslog group by hour order by hour"
        Waiting on bqjob_r18b08ce0f934b7f0_000001747613b256_1 ... (0s) Current status: DONE   
        +------+----------+
        | hour | hitcount |
        +------+----------+
        |    0 |    26983 |
        |    1 |    12287 |
        |    2 |     8824 |
        |    3 |     6607 |
        |    4 |    10519 |
        |    5 |    14581 |
        |    6 |    26634 |
        |    7 |    73708 |
        |    8 |   218842 |
        |    9 |   219769 |
        |   10 |   115119 |
        |   11 |    58151 |
        |   12 |    55623 |
        |   13 |    55625 |
        |   14 |    56057 |
        |   15 |    55675 |
        |   16 |    55573 |
        |   17 |    55740 |
        |   18 |    55800 |
        |   19 |    55935 |
        |   20 |    55996 |
        |   21 |    55797 |
        |   22 |    55778 |
        |   23 |    55755 |
        +------+----------+
    ```

*  Run another query

    *
    ```console
       student_03_0c36349759d9@cloudshell:~ (qwiklabs-gcp-03-e852ea611147)$ bq query "select string_field_10 as request, count(*) as requestcount from logdata.accesslog group by request order by reques
       tcount desc"
        Waiting on bqjob_r30f570fb61eee91_000001747615882e_1 ... (0s) Current status: DONE   
        +----------------------------------------+--------------+
        |                request                 | requestcount |
        +----------------------------------------+--------------+
        | GET /store HTTP/1.0                    |       337293 |
        | GET /index.html HTTP/1.0               |       336193 |
        | GET /products HTTP/1.0                 |       280937 |
        | GET /services HTTP/1.0                 |       169090 |
        | GET /products/desserttoppings HTTP/1.0 |        56580 |
        | GET /products/floorwaxes HTTP/1.0      |        56451 |
        | GET /careers HTTP/1.0                  |        56412 |
        | GET /services/turnipwinding HTTP/1.0   |        56401 |
        | GET /services/spacetravel HTTP/1.0     |        56176 |
        | GET /favicon.ico HTTP/1.0              |        55845 |
        +----------------------------------------+--------------+  
    ```          

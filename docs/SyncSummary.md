The Sync Summary Dataset
==================

The Sync Summary dataset is generated by `src/main/scala/com/mozilla/telemetry/views/SyncView.scala`. It contains one record for every entry in the `syncs` array in in the [sync ping](https://gecko.readthedocs.io/en/latest/toolkit/components/telemetry/telemetry/data/sync-ping.html).

Generating the dataset
======================

The dataset can be generated as follows:

1. SSH into a new spark cluster
2. `cd /mnt && git clone https://github.com/mozilla/telemetry-batch-view.git && cd telemetry-batch-view`
3. `sbt assembly`
4. `spark-submit --master yarn --deploy-mode client --class com.mozilla.telemetry.views.SyncView target/scala-2.11/telemetry-batch-view-1.1.jar --bucket telemetry-test-bucket --from $DATE --to $DATE`

Some adjustment to version numbers in step 4 may be necessary in the future.

Schema
======

The `sync_summary` dataset schema is as follows:
```
root
 |-- app_build_id: string (nullable = true)
 |-- app_display_version: string (nullable = true)
 |-- app_name: string (nullable = true)
 |-- app_version: string (nullable = true)
 |-- app_channel: string (nullable = true)
 |-- uid: string
 |-- deviceID: string (nullable = true)
 |-- when: integer
 |-- took: integer
 |-- why: string (nullable = true)
 |-- failureReason: struct (nullable = true)
 |    |-- name: string
 |    |-- value: string (nullable = true)
 |-- status: struct (nullable = true)
 |    |-- sync: string (nullable = true)
 |    |-- status: string (nullable = true)
 |-- devices: array (nullable = true)
 |    |-- element: struct (containsNull = false)
 |    |    |-- id: string
 |    |    |-- os: string
 |    |    |-- version: string
 |-- engines: array (nullable = true)
 |    |-- element: struct (containsNull = false)
 |    |    |-- name: string
 |    |    |-- took: integer
 |    |    |-- status: string (nullable = true)
 |    |    |-- failureReason: struct (nullable = true)
 |    |    |    |-- name: string
 |    |    |    |-- value: string (nullable = true)
 |    |    |-- incoming: struct (nullable = true)
 |    |    |    |-- applied: integer
 |    |    |    |-- failed: integer
 |    |    |    |-- newFailed: integer
 |    |    |    |-- reconciled: integer
 |    |    |-- outgoing: array (nullable = true)
 |    |    |    |-- element: struct (containsNull = false)
 |    |    |    |    |-- sent: integer
 |    |    |    |    |-- failed: integer
 |    |    |-- validation: struct (containsNull = false)
 |    |    |    |-- version: integer
 |    |    |    |-- checked: integer
 |    |    |    |-- took: integer
 |    |    |    |-- failureReason: struct (nullable = true)
 |    |    |    |    |-- name: string
 |    |    |    |    |-- value: string (nullable = true)
 |    |    |    |-- problems: array (nullable = true)
 |    |    |    |    |-- element: struct (containsNull = false)
 |    |    |    |    |    |-- name: string
 |    |    |    |    |    |-- count: integer

```

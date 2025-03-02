# Job States

When working with Handler Functions in SubModel, understanding the various states a job transitions through—from initiation to completion—is crucial. Each state provides valuable insights into the job's current status, enabling effective management of the job flow.

## Job States

Here are the possible states a job can be in:

- **IN_QUEUE**:  
  The job is currently in the endpoint queue, awaiting an available worker to pick it up for processing.

- **IN_PROGRESS**:  
  Once a worker picks up the job, its state changes to `IN_PROGRESS`. This indicates that the job is actively being processed and is no longer in the queue.

- **COMPLETED**:  
  When the job successfully finishes processing and returns a result, it transitions to the `COMPLETED` state. This signifies the successful execution of the job.

- **FAILED**:  
  If a job encounters an error during execution and returns with an error, it is marked as `FAILED`. This state indicates that the job did not complete successfully due to encountered issues.

- **CANCELLED**:  
  Jobs can be manually cancelled using the `/cancel/job_id` endpoint. If a job is cancelled before it completes or fails, it will be in the `CANCELLED` state.

- **TIMED_OUT**:  
  This state occurs in two scenarios:  
  1. When a job expires before a worker picks it up.  
  2. If the worker fails to report back a result for the job before it reaches its timeout threshold.  

Understanding these states ensures better tracking, troubleshooting, and management of jobs within the SubModel framework.
# Sending a Job Request

This guide outlines the process of sending a job request to an endpoint using a JSON body. Below are the key components:

1. **JSON Request Body**:
   - Your request must include an `input` key. For instance:
     ```json
     {
       "input": {
         "prompt": "The lazy brown fox jumps over the"
       }
     }
     ```

2. **Optional Inputs**:
   - Additional keys such as `webhook`, `execution policies`, and `S3-compatible storage` can be included.

3. **Webhooks**:
   - To receive notifications upon job completion, include a `webhook` URL in the request body:
     ```json
     {
       "input": {},
       "webhook": "https://URL.TO.YOUR.WEBHOOK"
     }
     ```
   - Ensure your webhook endpoint returns a `200` status code.

4. **Execution Policies**:
   - Control the job's execution with parameters like:
     - `executionTimeout`: Maximum duration for job completion.
     - `lowPriority`: Assigns a lower priority to the job.
     - `ttl`: Time the job can remain in the queue before termination.

5. **S3-Compatible Storage**:
   - To set up S3-compatible storage, include the credentials in the request:
     ```json
     {
       "input": {},
       "s3Config": {
         "accessId": "key_id_or_username",
         "accessSecret": "key_secret_or_password",
         "bucketName": "storage_location_name",
         "endpointUrl": "storage_location_address"
       }
     }
     ```
   - Note that this configuration is passed to the worker but is not included in the job output.

This structure ensures efficient job processing and facilitates effective tracking and management of job lifecycles.
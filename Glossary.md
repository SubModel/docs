# Glossary

## Serverless
Serverless is a pay-per-second computing solution designed for dynamic autoscaling in production environments. It automatically adjusts computational resources based on your request traffic, ensuring cost-effective usage.

We offer GPU and CPU Serverless options:
- **GPU Serverless**: Each worker is equipped with a dedicated GPU, ideal for AI/ML workloads.
- **CPU Serverless**: Workers come with high-clock-speed CPU cores, suited for general-purpose workloads.

## Worker
A single compute resource that processes requests. Each endpoint can have multiple workers, enabling parallel processing of multiple requests simultaneously.

## Total Workers
Refers to the maximum number of workers available to your account. The sum of the max workers assigned across all your endpoints cannot exceed this limit. If you run out of total workers, please reach out to us by [creating a support ticket]().

## Max Workers
Sets the upper limit on the number of workers your endpoint can run simultaneously.

**Default**: 3

## Active (Min) Workers
"Always on" workers. Setting active workers to 1 or more ensures that a worker is always ready to respond to job requests without cold start delays.

**Default**: 0  
**Note**: Active workers incur charges as soon as you enable them (set to >0), but they come with a discount of up to 30% off the regular price.

## Flex Workers
"Sometimes on" workers that help scale your endpoint during traffic surges. They are often referred to as idle workers since they spend most of their time in an idle state. Once a flex worker completes a job, it transitions to idle or sleep mode to save costs.

**Default**: Max Workers(3) - Active Workers(0) = 3

## Extra Workers
RunPod caches your worker's Docker image on our host servers, ensuring faster scalability. If you experience a traffic spike, you can increase the max number of workers, and extra workers will be immediately added as part of the flex workers to handle the increased demand.

**Default**: 2

## Worker States
- **Initializing**: When you create a new endpoint or release an update, RunPod needs to download and prepare the Docker image for your workers. During this process, workers remain in an initializing state until they are fully ready to handle requests.
- **Idle**: A worker is ready to handle new requests but is not actively processing any. There is no charge while a worker is idle.
- **Running**: A running worker is actively processing requests, and you are billed every second it runs. If a worker runs for less than a full second, it will be rounded up to the next whole second. For example, if a worker runs for 2.5 seconds, you will be billed for 3 seconds.
- **Throttled**: Sometimes, the machine where the worker is cached may be fully occupied by other workloads. In this case, the worker will show as throttled until resources become available.
- **Outdated**: When you update your endpoint configuration or deploy a new Docker image, existing workers are marked as outdated. These workers will continue processing their current jobs but will be gradually replaced through a rolling update.
- **Unhealthy**: When your container crashes, it’s usually due to a bad Docker image, an incorrect start command, or occasionally a machine issue. The system will automatically retry the unhealthy worker.

## Endpoint
Refers to a specific REST API (URL) provided by RunPod that your applications or services can interact with.

## Handler
A function you create that takes in submitted inputs, processes them (like generating images, text, or audio), and returns the final output.

## Serverless SDK
A Python package used when creating a handler function. This package helps your code receive requests from our serverless system, triggers your handler function to execute, and returns the function’s result back to the serverless system.

## Endpoint Settings
- **Idle Timeout**: The amount of time a worker remains running after completing its current request.
  **Default**: 5 seconds
- **Execution Timeout**: The maximum time a job can run before the system terminates the worker.
  **Default**: 600 seconds (10 minutes)
- **Job TTL**: Defines the maximum time a job can remain in the queue before it's automatically terminated.
  **Default**: 86,400,000 milliseconds (24 hours)

## Flashboot
RunPod’s solution for reducing the average cold-start times on your endpoint.

## Scale Type
- **Queue Delay**: Adjusts worker numbers based on request wait times.
- **Request Count**: Adjusts worker numbers according to total requests in the queue and in progress.

## Expose HTTP/TCP Ports
Allows direct communication with your worker using its public IP and port, useful for real-time applications requiring minimal latency.

## Endpoint Metrics
- **Requests**: Displays the total number of requests received by your endpoint and their statuses.
- **Execution Time**: Displays the P70, P90, and P98 execution times for requests on your endpoint.
- **Delay Time**: Displays the time requests spend waiting in the queue.
- **Cold Start Time**: Measures how long it takes to wake up a worker.
- **Cold Start Count**: Displays the number of cold starts during a given period.
- **WebhookRequest Responses**: Displays the number of webhook requests sent and their responses.

## Pod
- **Secure Cloud**: GPU instances running in T3/T4 data centers, providing high reliability and security.
- **Community Cloud**: GPU instances connecting individual compute providers to consumers through a secure peer-to-peer system.

## Datacenter
A secure location where RunPod's cloud computing services, such as Secure Cloud and GPU Instances, are hosted.

## GPU Instance
A container-based GPU instance that you can deploy within seconds.

## Template
A RunPod template is a Docker container image paired with a configuration.

## SDKs
RunPod provides several SDKs to interact with the RunPod platform.

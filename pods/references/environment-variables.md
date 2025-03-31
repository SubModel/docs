# Pod environment variables

You can store the following environment variables in your Pods.

| Variable               | Description                                                                 |
|------------------------|-----------------------------------------------------------------------------|
| `SUBMODEL_POD_ID`         | The unique identifier for your pod.                                         |
| `SUBMODEL_API_KEY`        | Used to make SubModel API calls to the specific pod. It's limited in scope to only the pod. |
| `SUBMODEL_POD_HOSTNAME`   | Name of the host server the pod is running on.                              |
| `SUBMODEL_GPU_COUNT`      | Number of GPUs available to the pod.                                        |
| `SUBMODEL_CPU_COUNT`      | Number of CPUs available to the pod.                                        |
| `SUBMODEL_PUBLIC_IP`      | If available, the publicly accessible IP for the pod.                       |
| `SUBMODEL_TCP_PORT_22`    | The public port SSH port 22.                                                |
| `SUBMODEL_DC_ID`          | The data center where the pod is located.                                   |
| `SUBMODEL_VOLUME_ID`      | The ID of the volume connected to the pod.                                  |
| `CUDA_VERSION`          | The installed CUDA version.                                                |
| `PWD`                  | Current working directory.                                                 |
| `PYTORCH_VERSION`      | Installed PyTorch Version.                                                 |
| `PUBLIC_KEY`           | The SSH public keys to access the pod over SSH.                             |
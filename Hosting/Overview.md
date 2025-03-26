# Vendor Cooperation Manual

## SubModel Resource Hosting Process and Recharge Settlement Introduction

Applicable for integrating self-owned servers, GPU devices, or third-party cloud resources into the SubModel platform for unified management.

### 1. Preparations

#### 1.1 Work Node:
- Sufficient external network bandwidth (at least 1Gbps)
- Operating System: Linux Ubuntu 22.04 LTS clean system
- Install required tools: Docker, NVIDIA drivers (CUDA required for GPU resources)
- Provide SSH root access
- Hardware: Support for virtualization (KVM/GPU passthrough, etc.)

#### 1.2 Optimized Resources (Recommended):
- If there is a storage matrix server or distributed storage service available within the intranet, provide the corresponding access method (paid network storage can be offered to users).
- It is recommended to arrange a virtual machine or a regular work server within the intranet with large storage (at least 10G) and a bandwidth of over 10Gbps connected to the work node. This is used to set up an intranet image proxy service (this greatly impacts scheduling performance and avoids frequent image pulls from the internet).

### 2. Resource Integration

#### 2.1 Self-Hosting Management
Self-hosting is suitable for users who own hardware resources and wish to retain full control. Users log in to the platform to integrate devices independently. They only need to download the client for the corresponding operating system and follow the command-line instructions.

#### 2.2 Delegating SSH Access for Management by the Operating Enterprise
This is suitable for users who wish to entrust a third-party operations team to fully manage the machines. Users need to generate an SSH key pair, provide the public key to the operating enterprise, and configure server SSH access. This operation requires signing a service agreement contract to clarify the scope and permissions of operations.

### 3. Recharge and Settlement

#### 3.1 Recharge
Currently, only crypto payment interfaces are supported. In the future, other payment interfaces such as credit cards and PayPal will be supported. Different recharge discounts will be adjusted according to different channels to meet the diverse needs of users.

*Note: If users (B2B users only) need to recharge using RMB, they can sign a contract with us and make a public-to-public transfer. We will recharge the user’s account through the backend.*

#### 3.2 Settlement Rules
The platform's settlement rules are real-time. Fees are deducted in real-time after consumption. For time-based billing products, fees are deducted once. For usage-based billing products, the minimum billing unit is 1 hour.

- **Credit Settlement**: A settlement method based on points or balance calculation, with a value of 1:1 to USD.
- **RMB Settlement**: If users need to settle in RMB, they can sign a settlement contract with us and make RMB transfers. The settlement cycle must be on a quarterly basis.

**Business Cooperation Matchmaking Email**：
📧 [business@submodel.ai](mailto:business@submodel.ai)
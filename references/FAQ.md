# FAQ

## How does Submodel work?
Submodel leverages technologies like [Docker](https://www.docker.com/) to containerize and isolate guest workloads on a host machine. We have built a decentralized platform where thousands of servers can be connected to offer a seamless experience for all users.

## Where can I go for help?
We'd be happy to help! Join our community on [Discord](https://discord.com/invite/UYn6rESDSC), message us in our support chat, or email us at admin@submodel.ai.

## What is Submodel's policy on refunds and credits?
If you aren't sure if Submodel is for you, feel free to hang out in our [Discord](https://discord.com/invite/UYn6rESDSC) to ask questions or email admin@submodel.ai You can load as little as $10 into your account to try things out. We don't currently offer refunds or trial credits due to the overhead of processing these requests. Please plan accordingly!

## What are our products?
View detailed information about our products and services here.[ [English](https://submodel-gpu-cloud-gkztjf0.gamma.site/) | [中文](https://submode-gpu-cloud-j3p6gm5.gamma.site/) ]

### Pod（On-Demand）
Users can access cloud resources in real time based on their needs and are billed per hour for the total CPU, memory, and GPU specifications used in the pod.

### Serverless
Developers no longer manage the server; the cloud platform automatically allocates resources, scales dynamically, and charges by execution time in seconds.

### Bare-Metal
Users exclusively occupy all resources of physical servers, experiencing zero virtualization performance loss while meeting ultra-low latency and hardware-level control needs

## Billing
All billing, including per-hour compute and storage billing, is charged per minute.

### How does Pod billing work?
Every Pod has an hourly cost based on GPU type. Your Submodel credits are charged for the Pod every minute as long as the Pod is running. If you ever run out of credits, your Pods will be automatically stopped, and you will get an email notification. Eventually, Pods will be terminated if you don't refill your credit. We pre-emptively stop all of your Pods if you get down to 10 minutes of remaining run time. This gives your account enough balance to keep your data volumes around in the case you need access to your data. Please plan accordingly.

Once a balance has been completely drained, all pods are subject to deletion at the discretion of the service. An attempt will be made to hold the pods for as long as possible, but this should not be relied upon!

### How does storage billing work?
**Storage Allocation for Pod Deployment**  
When deploying a new pod, you must allocate appropriate storage capacity. Each pod supports the following configurations:  
- **Container Disk**: 5 GB to 1,024 GB  
- **Volume Disk**: 0 GB to 1,024 GB  

Pricing for storage is standardized at **$0.005 per GB per day** for both storage types.  
**Important**: Memory upgrades for existing pods are not currently supported.  

### Can I withdraw my unused balance?
No, Submodel does not offer the option to withdraw your unused balance after depositing funds into your account. When you add funds to your Submodel account, these credits are non-refundable and can only be used for Submodel services.


> **IMPORTANT**
When depositing funds into your Submodel account, please be aware that you cannot withdraw your balance once it has been added. Only deposit the amount you intend to use for Submodel services.

We recommend carefully considering the amount you wish to deposit based on your expected usage of our services. If you have any questions about billing or need assistance in planning your Submodel expenses, please don't hesitate to contact our support team at admin@submodel.ai.

## Security

### Is my data protected from other clients?
Yes. Your data is run in a multi-tenant environment where other clients can't access your pod. For sensitive workloads requiring the best security, please use Secure Cloud.

### Is my data protected from the host of the machine my Pod is running on?
Data privacy is important to us at Submodel. Our Terms of Service prohibit hosts from trying to inspect your Pod data or usage patterns in any way. 

## Usability

### What can I do in a Submodel Pod?
You can run any Docker container available on any publicly reachable container registry. If you are not well versed in containers, we recommend sticking with the default run templates like our Submodel PyTorch template. However, if you know what you are doing, you can do a lot more!

### Can I run my own Docker daemon on Submodel?
You can't currently spin up your own instance of Docker, as we run Docker for you! Unfortunately, this means that you cannot currently build Docker containers on Submodel or use things like Docker Compose. Many use cases can be solved by creating a custom template with the Docker image that you want to run.

### My Pod is stuck on initializing. What gives?
Usually, this happens for one of several reasons. If you can't figure it out, [contact us](/references/contact-us.md), and we'll gladly help you.

- You are trying to run a Pod to SSH into, but you did not give the Pod an idle job to run like "sleep infinity."
- You have given your Pod a command that it doesn't know how to run. Check the logs to make sure that you don't have any syntax errors, etc.

### Can I run Windows?
We don't currently support Windows. We want to do this in the future, but we do not have a solid timeframe for Windows support.

### Why do I have zero GPUs assigned to my Pod?
Most of our machines have between 4 and 8 GPUs per physical machine. When you start a Pod, it is locked to a specific physical machine. If you keep it running (On-Demand), then that GPU cannot be taken from you. However, if you stop your Pod, it becomes available for a different user to rent. When you want to start your Pod again, your specific machine may be wholly occupied! In this case, we give you the option to spin up your Pod with zero GPUs so you can retain access to your data.

Remember that this does not mean there are no more GPUs of that type available, just none on the physical machine that specific Pod is locked to. Note that transfer Pods have limited computing capabilities, so transferring files using a UI may be difficult, and you may need to resort to terminal access or cloud sync options.

## What if?

### What if I run out of funds?
All your Pods are stopped automatically when you don't have enough funds to keep your Pods running for at least ten more minutes. When your Pods are stopped, your container disk data will be lost, but your volume data will be preserved. Pods are scheduled for removal if adequate credit balance is not maintained. If you fail to do so, your Pods will be terminated, and Pod volumes will be removed.

After you add more funds to your account, you can start your Pod if you wish (assuming enough GPUs are available on the host machine).

### What if the machine that my Pod is running loses power?
If the host machine loses power, it will attempt to start your Pod again when it returns online. Your volume data will be preserved, and your container will run the same command as it ran the first time you started renting it. Your container disk and anything in memory will be lost!

### What if my Pod loses internet connectivity?
The host machine continues to run your Pod to the best of its ability, even if it is not connected to the internet. If your job requires internet connectivity, then it will not function. You will not be charged if the host loses internet connectivity, even if it continues to run your job. You may, of course, request that your Pod exit while the host is offline, and it will exit your Pod when it regains network connectivity.

### What if it says that my spending limit has been exceeded?
We implement a spending limit for newer accounts that will grow over time. This is because we have found that sometimes scammers try to interfere with the natural workings of the platform. We believe that this limit should not impact normal usage. We would be delighted to up your spending limit if you contact us and share your use case.

## Legal

### Do you have some legal stuff I can look at?
Sure, do! Take a look at our legal page.

[Privacy Policy](https://submodel.ai/#/about/privacy)

[Terms of Service](https://submodel.ai/#/about/terms)

## GDPR Compliance

### Is Submodel compliant with GDPR for data processed in Europe?
Yes, Submodel is fully compliant with the General Data Protection Regulation (GDPR) requirements for any data processed within our European data center regions.

### What measures does Submodel take to ensure GDPR compliance?
For servers hosted in GDPR-compliant regions like the European Union, we ensure:

- **Data processing procedures**: We have established clear procedures for the collection, storage, processing, and deletion of personal data, ensuring transparency and accountability in our data processing activities.
- **Data protection measures**: We have implemented appropriate technical and organizational measures to safeguard personal data against unauthorized access, disclosure, alteration, and destruction.
- **Consent mechanisms**: We obtain and record consent from individuals for the processing of their personal data in accordance with GDPR requirements, and we provide mechanisms for individuals to withdraw consent if desired.
- **Rights of data subjects**: We facilitate the rights of data subjects under the GDPR, including the right to access, rectify, erase, or restrict the processing of their personal data, and we handle data subject requests promptly and efficiently.
- **Data transfer mechanisms**: We ensure lawful and secure transfer of personal data outside the EU, where applicable, in compliance with GDPR requirements, utilizing appropriate mechanisms such as adequacy decisions, standard contractual clauses, or binding corporate rules.
- **Compliance monitoring**: We regularly monitor and review our GDPR compliance to ensure ongoing effectiveness and adherence to regulatory requirements, conducting data protection impact assessments and internal audits as needed.

For any inquiries or concerns regarding our GDPR compliance or our data protection practices, reach out to our team through email at admin@submodel.ai.
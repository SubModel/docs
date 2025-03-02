# Create a Network Volume

Network Volumes are a specialized feature of Secure Cloud that enable multiple pods to interact with a single volume. This functionality offers enhanced flexibility, particularly when working with high-demand GPU pools that may not always be available. By creating a pod in a different pool while waiting for availability, you can maintain productivity. Additionally, Network Volumes save time by allowing you to store frequently used models or large files for later use, eliminating the need to re-download them each time you start a new pod.

## How to Create a Volume

To create a volume, navigate to the Secure Cloud page and select the option to create a volume.

![Volume creation](assets/images/797cfdc-image-6065ac7f873bef52b971d60c3f926990.png)

You will be presented with the pricing details for the volume. From there, you can choose a data center, provide a name, and specify the desired size for your volume.

![Volume setup](assets/images/596a4f5-image-14d555d8f1316bb57eed78b840a4126a.png)

Once the volume is created, it will appear in your list of volumes.

![Volume list](assets/images/c4eea31-image-84cf22fda878c9e419619ecf39143bcb.png)

When you click the Deploy option, your container size will be locked to the size of your network volume. Please note that there is a nominal cost for network volume storage, which replaces the standard disk cost. Additionally, you can link multiple pods to a single network volume, resulting in overall cost savings, especially when two or more pods share one volume instead of each having separate volumes.

![Deploy option](assets/images/8f69d41-image-7c106c3d43252c6096872bd9be8a0467.png)

## The Infrastructure Behind Network Volume

Creating a Network Volume grants you access to our advanced infrastructure, which includes state-of-the-art storage servers located in the same datacenters as our GPU servers. These servers are interconnected via a high-speed 25Gbps local network, with some locations offering up to 200Gbps, ensuring efficient data transfer and minimal latency. All data is stored on high-speed NVME SSDs to guarantee optimal performance.


**Please note that if your machine-based storage or network volume is terminated due to insufficient funds, the disk space is immediately made available for other clients, and SubModel cannot assist in recovering lost storage.**
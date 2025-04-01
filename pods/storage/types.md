# Storage Types

The following section outlines the various storage and volume options available.

## Container Volume

A container volume is a storage type that hosts the operating system and offers temporary storage for a Pod.  
It is generated upon Pod launch and is closely tied to the Pod's lifecycle.

**Key Characteristics:**

- Volatile storage that is erased if the Pod is stopped or restarted
- Ideal for temporary data or files that do not need to persist beyond the Pod's lifecycle
- Capacity is determined by the chosen Pod configuration
- Delivers fast read and write speeds due to its local attachment to the Pod

## Disk Volume

A disk volume is a form of persistent storage that is maintained throughout the Pod’s lease period. It operates like a hard disk, enabling the storage of data that must be retained even if the Pod is stopped or restarted. The disk volume is mounted at **/workspace**.

**Key Characteristics:**

- Persistent storage that remains accessible for the duration of the Pod's lease
- Suitable for data, models, or files that need to be preserved across Pod restarts or reconfigurations
- Capacity can be tailored to meet storage needs
- Ensures reliable data persistence, though read and write speeds may be slightly slower compared to container volumes


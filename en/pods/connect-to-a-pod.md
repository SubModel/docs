# Connect to a Pod

You can access a Pod using different methods, depending on your specific needs, preferences, and the templates in use.

## SSH terminal

Using an SSH terminal to connect to a Pod is a secure and dependable option, ideal for long-running tasks and critical operations.

Each Pod is equipped to allow SSH access.

To get started, ensure you have an SSH client installed on your local machine.

1. Open the terminal on your local device.
2. Select one of the following commands and enter it into your terminal:

    ```bash
    # No support for SCP & SFTP
    ssh <username>@<pod-ssh-hostname> -i <path-to-ssh-key>

    # Supports SCP & SFTP
    ssh <username>@<pod-ip-address> -p <ssh-port> -i <path-to-ssh-key>
    ```

Fill in the placeholders with the following information:

- `<username>`: The username assigned to you for the Pod
- `<pod-ssh-hostname>`: The SSH hostname provided for your Pod
- `<pod-ip-address>`: The IP address of your Pod
- `<ssh-port>`: The designated SSH port for your Pod
- `<path-to-ssh-key>`: The location of your SSH private key file

After this, you will have established a secure SSH connection to your Pod.

## Web terminal

> The ability to connect to a web terminal depends on your Pod’s template.

The web terminal provides a fast and convenient way to connect to your Pod and execute commands. However, it’s not recommended for long-running tasks like training an LLM or other high-priority jobs. It is designed for quick logins and simple tasks.

1. On your Pod's page, click on **Connect**.
2. Choose **Start Web Terminal** and then select **Connect to Web Terminal** in a new window.
3. Enter the **Username** and **Password**.

> You can find the **Username** and **Password** for your web terminal once you click **Start Web Terminal**.
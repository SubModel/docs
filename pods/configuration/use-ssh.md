# Use SSH

The basic terminal SSH access provided by SubModel is not a full SSH connection and therefore does not support commands like SCP. To enable full SSH capabilities, you need to rent an instance with public IP support and run a full SSH daemon in your Pod.

## Setup

1. **Generate SSH Key Pair**: On your local machine, generate a public/private SSH key pair using the command:
   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```
   This will save your public key to `~/.ssh/id_ed25519.pub` and your private key to `~/.ssh/id_ed25519`.

   **Note**: If you're using Command Prompt in Windows (not WSL or Linux terminal), your keys will be saved to:
   ```
   C:\users\{yourUserAccount}\.ssh\id_ed25519.pub
   C:\users\{yourUserAccount}\.ssh\id_ed25519
   ```

   ![SSH Key Generation](/assets/images/4655a01-1-6b0dd022d29cebe257e9e5df21a52fb4.png)

2. **Add Public Key to SubModel**: Log in to your [SubModel user settings](https://www.SubModel.io/console/user/settings) and add your public key.

   ![SubModel User Settings](/assets/images/4972691-2-8c4764cae5d826dd756d264474f818d5.png)

   ![Public Key Addition](/assets/images/c340553-image-5834fe29a2179e6bb7fb75f5aa664037.png)

3. **Start Your Pod**: Ensure the following:
   - Your Pod supports a public IP (if deploying in Community Cloud).
   - An SSH daemon is running. If using a SubModel official template (e.g., SubModel Stable Diffusion), no additional steps are needed. For custom templates, ensure TCP port 22 is exposed and use the following Docker command. Replace `sleep infinity` with your existing command if applicable:
     ```bash
     bash -c 'apt update; DEBIAN_FRONTEND=noninteractive apt-get install openssh-server -y; mkdir -p ~/.ssh; cd $_; chmod 700 ~/.ssh; echo "$PUBLIC_KEY" >> authorized_keys; chmod 700 authorized_keys; service ssh start; sleep infinity'
     ```

   ![Pod Initialization](/assets/images/97823c6-image-73915734bbfd2cd60e89c719561877e1.png)

Once your Pod initializes, you can SSH into it using the SSH over exposed TCP command from the Pod's Connection Options menu.

**Note**:
- If using Windows Command Prompt (not WSL or Linux terminal) and you used the default key location, modify the file path in the SSH command after the `-i` flag to:
  ```
  C:\users\{yourUserAccount}\.ssh\id_ed25519
  ```
- If you saved your key to a non-default location, specify that path after the `-i` flag.

![SSH Connection](/assets/images/3d51ed8-image-e3c4caabde1ffd0116215fcedd5986d8.png)

![SSH Connection Options](/assets/images/ff71847-image-7ce565e90197a0386f790c9f8a987987.png)

## What's the SSH Password?

If prompted for a password during connection, something is wrong. SubModel does not require a password for SSH connections. Common issues include:

- Copying the key **fingerprint** (starting with `SHA256:`) instead of the public key (contents of `id_ed25519.pub`).
- Omitting the encryption type (`ssh-ed25519`) when copying the public key.
- Not separating multiple public keys with a newline in SubModel user settings.
- Specifying an incorrect file path to your private key:

  ![Incorrect File Path](/assets/images/10cbfa6-image-3f9272d8e4fcad6a389624dacca81776.png)

- Using a private key with incorrect permissions:

  ![Permissions Issue](/assets/images/7a5cf85-image-c96832adff89df1939a8549b42d4bd74.png)

- Incorrect private key specified in the SSH config file. Ensure the `IdentityFile` in your `~/.ssh/config` points to the correct private key. A mismatch will prompt for a password.

  ![Private Key Fix](https://github.com/SubModel/docs/assets/19496114/1f3db241-72a1-4d29-be36-ea5bab945b0a)
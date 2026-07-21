# 使用 SSH

SubModel 提供的基础终端 SSH 访问并不是完整的 SSH 连接,因此不支持 SCP 之类的命令。要启用完整的 SSH 能力,你需要租用一个支持公网 IP 的实例,并在 Pod 中运行一个完整的 SSH 守护进程(daemon)。

## 设置

1. **生成 SSH 密钥对**:在本地机器上,使用以下命令生成一对公钥/私钥:
   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```
   这会将公钥保存到 `~/.ssh/id_ed25519.pub`,私钥保存到 `~/.ssh/id_ed25519`。

   **注意**:如果你在 Windows 中使用命令提示符(Command Prompt,而非 WSL 或 Linux 终端),密钥会保存到:
   ```
   C:\users\{yourUserAccount}\.ssh\id_ed25519.pub
   C:\users\{yourUserAccount}\.ssh\id_ed25519
   ```

   ![SSH Key Generation](/assets/images/4655a01-1-6b0dd022d29cebe257e9e5df21a52fb4.png)

2. **将公钥添加到 SubModel**:登录你的 [SubModel 用户设置](https://submodel.ai/#/account/others)并添加你的公钥。

   ![SubModel User Settings](/assets/images/4972691-2-8c4764cae5d826dd756d264474f818d5.png)

   ![Public Key Addition](/assets/images/c340553-image-5834fe29a2179e6bb7fb75f5aa664037.png)

3. **启动你的 Pod**:确保以下几点:
   - 你的 Pod 支持公网 IP(如果部署在 Community Cloud)。
   - 有一个 SSH 守护进程在运行。如果使用 SubModel 官方 Template(例如 SubModel Stable Diffusion),则无需额外步骤。对于自定义 Template,请确保暴露了 TCP port 22,并使用以下 Docker 命令。如有需要,可将 `sleep infinity` 替换为你已有的命令:
     ```bash
     bash -c 'apt update; DEBIAN_FRONTEND=noninteractive apt-get install openssh-server -y; mkdir -p ~/.ssh; cd $_; chmod 700 ~/.ssh; echo "$PUBLIC_KEY" >> authorized_keys; chmod 700 authorized_keys; service ssh start; sleep infinity'
     ```

   ![Pod Initialization](/assets/images/97823c6-image-73915734bbfd2cd60e89c719561877e1.png)

Pod 初始化完成后,你可以从 Pod 的连接选项(Connection Options)菜单中使用 SSH over exposed TCP 命令通过 SSH 登录。

**注意**:
- 如果你使用的是 Windows 命令提示符(而非 WSL 或 Linux 终端),且使用了默认的密钥位置,请将 SSH 命令中 `-i` 标志后的文件路径修改为:
  ```
  C:\users\{yourUserAccount}\.ssh\id_ed25519
  ```
- 如果你把密钥保存在了非默认位置,请在 `-i` 标志后指定该路径。

![SSH Connection](/assets/images/3d51ed8-image-e3c4caabde1ffd0116215fcedd5986d8.png)

![SSH Connection Options](/assets/images/ff71847-image-7ce565e90197a0386f790c9f8a987987.png)

## SSH 密码是什么?

如果连接时被提示输入密码,说明出了问题。SubModel 的 SSH 连接不需要密码。常见问题包括:

- 复制了密钥**指纹**(以 `SHA256:` 开头)而不是公钥(`id_ed25519.pub` 的内容)。
- 复制公钥时遗漏了加密类型(`ssh-ed25519`)。
- 在 SubModel 用户设置中,多个公钥之间没有用换行分隔。
- 指定了错误的私钥文件路径:

  ![Incorrect File Path](/assets/images/10cbfa6-image-3f9272d8e4fcad6a389624dacca81776.png)

- 使用了权限不正确的私钥:

  ![Permissions Issue](/assets/images/7a5cf85-image-c96832adff89df1939a8549b42d4bd74.png)

- SSH 配置文件中指定了错误的私钥。请确保 `~/.ssh/config` 中的 `IdentityFile` 指向正确的私钥。不匹配会导致提示输入密码。

  ![Private Key Fix](https://github.com/SubModel/docs/assets/19496114/1f3db241-72a1-4d29-be36-ea5bab945b0a)

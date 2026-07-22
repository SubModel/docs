# 传输文件

学习如何在本地与 SubModel 之间传输文件。

## 前提条件

* 如果你打算使用 `scp`,请确保你的 Pod 已配置为使用真正的 SSH。更多信息,请参见 [使用 SSH](https://github.com/SubModel/docs/tree/main/pods/configuration/use-ssh.md)。
* 如果你打算使用 `rsync`,请确保本地机器和 Pod 上都已通过 `apt install rsync` 安装它。
* 记下 SSH over exposed TCP 命令中的公网 IP 地址和外部 port(SCP/rsync 命令会用到)。

## 使用 SCP 传输

使用 SCP 向 Pod 发送文件的一般语法如下(在本地机器上执行,并将其中的 x 替换为你 Pod 的外部 TCP port 和 IP;本例中分别为 43201 和 194.26.196.6):

```shell
scp -P 43201 -i ~/.ssh/id_ed25519 /local/file/path root@194.26.196.6:/destination/file/path
```

> **注意** 如果你的私钥文件不在 `~/.ssh/id_ed25519`,或者你使用的是 Windows 命令提示符,请在命令中相应地更新该路径。

向 Pod 发送文件的示例:

```shell
scp -P 43201 -i ~/.ssh/id_ed25519 ~/documents/example.txt root@194.26.196.6:/root/example.txt
```

如果你想从 Pod 接收文件,交换源参数和目标参数即可:

```shell
scp -P 43201 -i ~/.ssh/id_ed25519 root@194.26.196.6:/root/example.txt ~/documents/example.txt
```

如果你需要传输一个目录,使用 `-r` 标志来递归复制文件和子目录(这也会跟随遇到的任何符号链接):

```shell
scp -r -P 43201 -i ~/.ssh/id_ed25519 ~/documents/example_dir root@194.26.196.6:/root/example_dir
```

## 使用 rsync 传输

> **注意** 你的本地机器必须运行 Linux 或 [WSL 实例](https://learn.microsoft.com/en-us/windows/wsl/about),才能使用 rsync。

使用 rsync 向 Pod 发送文件的一般语法如下(在本地机器上执行,并将其中的 x 替换为你 Pod 的外部 TCP port 和 IP):

```shell
rsync -e "ssh -p 43201" /source/file/path root@194.26.196.6:/destination/file/path
```

一些有用的标志包括:

* `-a`/`--archive` - 归档模式(确保在传输过程中保留权限、时间戳和其他属性;传输目录或其内容时使用它)
* `-d`/`--delete` - 删除目标目录中源目录里没有的文件
* `-p`/`--progress` - 显示文件传输进度
* `-v`/`--verbose` - 详细输出
* `-z`/`--compress` - 在发送时压缩数据、接收时解压(会增加 CPU 负担,但对网络连接更友好)

使用 rsync 向 Pod 发送文件的示例:

```shell
rsync -avz -e "ssh -p 43201" ~/documents/example.txt root@194.26.196.6:/root/example.txt
```

如果你想从 Pod 接收文件,交换源参数和目标参数即可:

```shell
rsync -avz -e "ssh -p 43201" root@194.26.196.6:/root/example.txt ~/documents/example.txt
```

要传输一个目录的内容(而不传输目录本身),在文件路径末尾加上斜杠:

```shell
rsync -avz -e "ssh -p 43201" ~/documents/example_dir/ root@194.26.196.6:/root/example_dir/
```

如果末尾不加斜杠,则会传输目录本身:

```shell
rsync -avz -e "ssh -p 43201" ~/documents/example_dir root@194.26.196.6:/root/
```

rsync 的一个优点是,如果你尝试两次复制同样的文件,已经存在于目标位置的文件不会被再次传输(注意第二次执行后传输的数据量极小):

```shell
rsync -avz -e "ssh -p 43201" ~/documents/example.txt root@194.26.196.6:/root/example.txt
sending incremental file list
example.txt
             119 100%    0.00kB/s    0:00:00 (xfr#1, to-chk=0/1)

sent 243 bytes  received 35 bytes  185.33 bytes/sec
total size is 119  speedup is 0.43

$ rsync -avz -e "ssh -p 43201" ~/documents/example.txt root@194.26.196.6:/root/example.txt
sending incremental file list

sent 120 bytes  received 12 bytes  88.00 bytes/sec
total size is 119  speedup is 0.90
```

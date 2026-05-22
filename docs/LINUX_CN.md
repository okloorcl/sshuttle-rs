# Linux 后端说明（中文）

[English](LINUX_EN.md)

## 范围

Linux 透明模式使用平台重定向规则，加本地 TCP/DNS/UDP worker 实现接管。

## 后端

- `iptables` / `ip6tables`
- `nft`
- `auto` 自动选择

## 流量路径

- TCP 重定向到本地透明监听端口。
- DNS UDP 可通过 `--dns-capture` 捕获。
- 指定 UDP 端口可通过 `--udp-capture --udp-port ...` 捕获。

## 绕过

Linux 进程绕过基于 owner match：

```bash
sudo sshuttle-rs run \
  --platform linux \
  --mode transparent \
  --proxy 127.0.0.1:1080 \
  --proxy-type socks5 \
  --bypass-uid 1001 \
  --bypass-gid 1001
```

最稳的用法是让上游代理客户端用独立用户运行，然后绕过该 UID/GID。

## Policy

启动时会校验 policy 文件。Linux 当前主要把 policy 用于校验和解释；内核层执行仍基于 include/exclude CIDR 与 UID/GID 绕过。

## 清理

异常退出后使用 `cleanup`：

```bash
sudo sshuttle-rs cleanup --platform linux --mode transparent
```

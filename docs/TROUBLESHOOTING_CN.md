# 故障排查（中文）

[English](TROUBLESHOOTING_EN.md)

## Windows 透明模式无法启动

检查：

- 是否用管理员权限运行。
- `WinDivert.dll` 与 `WinDivert64.sys` / `WinDivert32.sys` 是否和 `sshuttle-rs.exe` 在同一目录。
- 部分安全软件可能阻止 WinDivert 驱动加载。

运行：

```powershell
sshuttle-rs.exe doctor --platform windows --mode transparent
```

## 代理客户端自己断线

通常是代理客户端自身流量被接管了。

使用：

```powershell
sshuttle-rs.exe run --platform windows --bypass-process "your-proxy-client.exe"
```

或使用 policy：

```yaml
rules:
  - name: bypass-proxy-client
    action: bypass
    priority: 100
    process:
      name: "your-proxy-client.exe"
```

## Linux 异常退出后规则残留

运行 cleanup：

```bash
sudo sshuttle-rs cleanup --platform linux --mode transparent
```

## DNS 没有走代理

开启 DNS 捕获：

```bash
sshuttle-rs run --dns-capture --dns-via-socks
```

非 SOCKS5 上游对 DNS-over-proxy 有限制，因为 UDP associate 是 SOCKS5 特性。

## UDP 应用没有走代理

UDP 需要按端口显式开启：

```bash
sshuttle-rs run --udp-capture --udp-port 443 --udp-port 3478
```

这样可以避免误捕获高流量或局域网发现类 UDP 流量。

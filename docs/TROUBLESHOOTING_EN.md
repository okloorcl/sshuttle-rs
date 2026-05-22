# Troubleshooting (English)

[中文](TROUBLESHOOTING_CN.md)

## Windows Transparent Mode Does Not Start

Check:

- Run as Administrator.
- Ensure `WinDivert.dll` and `WinDivert64.sys` / `WinDivert32.sys` are beside `sshuttle-rs.exe`.
- Some endpoint security software may block WinDivert driver loading.

Run:

```powershell
sshuttle-rs.exe doctor --platform windows --mode transparent
```

## Proxy Client Loses Connection

This usually means the proxy client itself is being captured.

Use:

```powershell
sshuttle-rs.exe run --platform windows --bypass-process "your-proxy-client.exe"
```

Or use policy:

```yaml
rules:
  - name: bypass-proxy-client
    action: bypass
    priority: 100
    process:
      name: "your-proxy-client.exe"
```

## Linux Rules Remain After Crash

Run cleanup:

```bash
sudo sshuttle-rs cleanup --platform linux --mode transparent
```

## DNS Does Not Follow Proxy

Enable DNS capture:

```bash
sshuttle-rs run --dns-capture --dns-via-socks
```

For non-SOCKS5 upstreams, DNS-over-proxy is limited because UDP associate is SOCKS5-specific.

## UDP App Still Bypasses Proxy

UDP is opt-in by port:

```bash
sshuttle-rs run --udp-capture --udp-port 443 --udp-port 3478
```

This keeps the tool practical and avoids accidentally capturing high-volume or local discovery traffic.

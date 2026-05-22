# Linux Backend (English)

[中文](LINUX_CN.md)

## Scope

Linux transparent mode uses platform packet redirection rules plus local TCP/DNS/UDP workers.

## Backends

- `iptables` / `ip6tables`
- `nft`
- `auto` selection

## Traffic Paths

- TCP is redirected to the local transparent listener.
- DNS UDP can be captured with `--dns-capture`.
- Selected UDP ports can be captured with `--udp-capture --udp-port ...`.

## Bypass

Linux process bypass is implemented with owner matching:

```bash
sudo sshuttle-rs run \
  --platform linux \
  --mode transparent \
  --proxy 127.0.0.1:1080 \
  --proxy-type socks5 \
  --bypass-uid 1001 \
  --bypass-gid 1001
```

For the most reliable setup, run the upstream proxy client under a dedicated user and bypass that UID/GID.

## Policy

Policy files are validated on startup. Linux currently uses policy for validation and explainability; kernel-level enforcement is still based on include/exclude CIDR plus UID/GID bypass.

## Cleanup

Use `cleanup` after abnormal exits:

```bash
sudo sshuttle-rs cleanup --platform linux --mode transparent
```

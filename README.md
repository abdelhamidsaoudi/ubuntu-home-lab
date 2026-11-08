# ubuntu-home-lab
Isolated Linux and home-network lab on an Ubuntu Server VM (Parallels, Apple Silicon)

## Setup

| Item | Detail |
|---|---|
| Host | MacBook Air, Apple Silicon |
| Hypervisor | Parallels |
| Guest OS | Ubuntu Server 26.04.1 LTS |
| Kernel | 7.0.0-42-generic |
| Architecture | aarch64 (ARM64), same as the host chip |

Verified with `uname -a`:

`Linux ubuntuserverlab 7.0.0-42-generic ... aarch64 GNU/Linux`

## Goals
- Terminal basics, files and permissions, processes
- Key-based SSH
- Basic networking (`ip a`, `ping`, `curl`)
- Keep the lab isolated from my home Wi-Fi (no bridged networking)

## Network

**Mode:** Parallels *Shared Network* (NAT). I deliberately did not use either Bridged option, so the VM is not placed directly on my home network. Parallels also offers *Host-Only*, which would cut the VM off from the internet, so I didn't use it either.

| Device | Interface | Address |
|---|---|---|
| Ubuntu Server VM | `enp0s5` | `10.211.55.6/24` |
| MacBook (Wi-Fi) | `en0` | `192.168.218.124` |

The VM and the Mac are on **different networks** (`10.211.55.x` and `192.168.218.x`). The Mac sits between them and passes the VM's traffic to the internet (NAT). Because of that, other devices on my Wi-Fi, such as my phone, can't open a connection to the VM directly. The VM can still reach out.

**Connectivity test:** `ping 8.8.8.8` from the VM

- 11 packets sent, 11 received, 0% packet loss
- Round-trip time (latency): min 53.6 ms, avg 66.5 ms, max 76.3 ms
- This confirms the VM has working outbound internet access through the Mac.

## Lessons learned
- In `ip a`, the address after `brd` is the *broadcast* address, not the machine's own. The machine's address is the one right after `inet`.
- `ping` time measures latency (how long a round trip takes), not bandwidth (how much data per second).
- A VM's internet access comes from the hypervisor's network mode, not from the guest joining Wi-Fi. That's why the Settings app showed "No Wi-Fi Adapter Found".

## Progress
- [x] Repo created
- [x] Ubuntu Server VM installed and verified
- [x] Network mode verified
- [ ] Terminal basics (next)

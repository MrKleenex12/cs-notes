# SSH (Secure Shell)

## Overview

SSH is a **cryptographic network protocol** for operating network services securely over an unsecured network. It is a secure alternative to unprotected login protocols (Telnet, rlogin) and insecure file transfer methods such as FTP. SSH is often used for logging into servers remotely, managing infrastructure, and transferring files.

SSH uses **public-key cryptography** to authenticate the remote computer (the server) and, optionally, the user. The encryption itself uses fast symmetric keys that both sides agree on at the start of each connection.

SSH is split into three layers, each handling a different part of security:

1. **Transport layer** – encryption, integrity, and server authentication
2. **User authentication layer** – proving who the user is
3. **Connection layer** – running shells, commands, file transfers, and port forwarding over the secure tunnel

## Problems to Solve

You are typing commands to a server. What attacks do you need to protect yourself from? How could someone maliciously see or alter your commands, or pretend to be you or the server?

| Goal | Protects against |
| --- | --- |
| **Confidentiality** | Attackers seeing your traffic |
| **Integrity** | Attackers modifying your traffic |
| **Authentication (server)** | Attackers pretending to be the server |
| **Authentication (you)** | Attackers pretending to be you |

---

## Layer 1: Transport

### Port 22

A **port** is a number that tells the operating system which program should receive incoming network traffic. One machine has **one IP address** but runs many services, so the port selects the service.

- SSH servers (`sshd`) listen on **TCP port 22** by default.
- TCP provides reliable, ordered delivery but **no security**. SSH adds all of that on top.
- Changing the port (e.g. `Port 2222` in `sshd_config`) reduces automated bot traffic in logs, but it is **not real security**, since a port scan finds it quickly.

### KEXINIT: Supported Algorithm Lists

The client and server each send a `KEXINIT` message listing the algorithms they support, ordered by preference.

`KEXINIT` is sent at the **start** of a session, and can be sent again in the **middle** of a session (**rekeying**) to get fresh keys. Rekeying limits how much traffic any single set of keys protects.

#### The 10 category lists

| Category | Lists |
| --- | --- |
| Key exchange method | 1 |
| Server host key algorithm | 1 |
| Cipher | 2 (client → server, server → client) |
| MAC | 2 (client → server, server → client) |
| Compression | 2 (client → server, server → client) |
| Language tags | 2 (client → server, server → client) |

The arrows mean the **direction the data travels**:

- **client → server**: what you send (keystrokes, commands)
- **server → client**: what the server sends back (output)

Each direction gets its own negotiated algorithm and **its own keys**, even when the algorithm is the same.

#### Choosing the algorithm

> **RFC 4253:** For each category, pick the first algorithm on the **client's** list that also appears anywhere on the **server's** list.

- The client's order decides. The server can only veto an algorithm by leaving it off its list.
- Example:

  ```text
  Client kex:  curve25519-sha256, ecdh-sha2-nistp256
  Server kex:  ecdh-sha2-nistp256, curve25519-sha256
  Chosen:      curve25519-sha256
  ```

#### No round trip needed

Neither the client nor the server waits on the other before sending `KEXINIT`. Both send at the same time, and because selection is **deterministic**, each side computes the same result independently once it has both lists.

#### Required categories

**Every required category must find a match. If even one fails, the whole connection fails.**

Required: key exchange, host key, cipher (both directions), MAC (both directions, **unless** the cipher is AEAD, which handles integrity itself), and compression (both; `none` is almost always supported).

Optional: language tags.

Example failure (modern client, old device):

```text
Unable to negotiate with 192.168.1.1 port 22: no matching key exchange method found.
Their offer: diffie-hellman-group1-sha1
```

#### Other KEXINIT details

- **Cookie:** 16 random bytes at the start of each `KEXINIT`, ensuring every session's exchange hash (and session ID) is unique.
- **Extension signals:** some list entries (e.g. `kex-strict-c-v00@openssh.com`) aren't real algorithms; they advertise support for protocol features. Strict key exchange was added to fix the **Terrapin attack** (2023).

---

## Useful Commands

```bash
ssh -vv user@host                                        # watch the negotiation happen
ssh -Q kex                                               # key exchange methods your client supports
ssh -Q cipher                                            # ciphers your client supports
sudo sshd -T | grep -E 'kexalgorithms|ciphers|macs'      # what your server offers
```

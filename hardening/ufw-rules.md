# UFW Firewall Rules — Atlas Server

## Default Policy

```bash
sudo ufw default deny incoming     # Block all incoming traffic
sudo ufw default allow outgoing    # Allow all outgoing traffic
```

## Open Ports

| Rule | Port | Protocol | Purpose |
|------|------|----------|---------|
| 1 | 22222 | TCP | SSH (non-standard port) |
| 2 | 80 | TCP | HTTP — Nginx Proxy Manager |
| 3 | 443 | TCP | HTTPS — Nginx Proxy Manager |

## Commands

```bash
# Apply rules
sudo ufw allow 22222/tcp comment 'SSH'
sudo ufw allow 80/tcp comment 'HTTP - NPM'
sudo ufw allow 443/tcp comment 'HTTPS - NPM'

# Enable firewall
sudo ufw enable

# Check status
sudo ufw status numbered

# Remove a rule by number
sudo ufw delete <number>
```

## Important: Docker and UFW

Docker manipulates iptables directly and can bypass UFW. To prevent services from being exposed unintentionally, all containers bind their ports to `127.0.0.1`:

```yaml
ports:
  - "127.0.0.1:8080:80"   # Only reachable locally
```

Nginx Proxy Manager is the only container that binds to `0.0.0.0` on ports 80 and 443.

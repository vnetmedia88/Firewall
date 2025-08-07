# FIREWALL ACCEPT AND DROP

Deskripsi singkat tentang project kamu di sini. Jelaskan fungsinya dan tujuannya.

## 🛠️ Instalasi

# 1. Reset semua rules
COPY
```bash
iptables -F
iptables -X
iptables -t nat -F
iptables -t nat -X
```

# 2. Set default policy

COPY

```bash

iptables -P INPUT DROP
iptables -P OUTPUT ACCEPT
iptables -P FORWARD ACCEPT
```

# 3. Izinkan koneksi terkait

COPY

```bash

iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```

# 4. Izinkan koneksi lokal (loopback)

```bash
iptables -A INPUT -i lo -j ACCEPT
```

# 5. Izinkan port yang dibutuhkan (CONTOH )

COPY

```bash

iptables -A INPUT -p tcp --dport 22 -j ACCEPT       # SSH
iptables -A INPUT -p tcp --dport 212 -j ACCEPT      # Custom port
iptables -A INPUT -p tcp --dport 51820 -j ACCEPT    # WireGuard
iptables -A INPUT -p tcp --dport 9443 -j ACCEPT     # Custom port
iptables -A INPUT -p tcp --dport 8000 -j ACCEPT     # Custom port
```

# 6. Izinkan port range 1000-1010 (CUSTOME)

COPY

```bash


for port in $(seq 1000 1010); do
  iptables -A INPUT -p tcp --dport $port -j ACCEPT
done
```

# 7. Install iptables-persistent jika belum ada

COPY

```bash


if ! dpkg -l | grep -q iptables-persistent; then
  echo "Installing iptables-persistent..."
  apt update && apt install -y iptables-persistent
fi
```

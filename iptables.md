# ✅ firewwal iptabless

## 1: jangan lupa matra sebelum intsall apapun di linux 😀😀

```bash
sudo apt update

```

## 2. Install iptables-persistent (jika belum):

```bash
sudo apt install iptables-persistent

```
Saat instalasi, biasanya akan muncul prompt untuk menyimpan aturan iptables sekarang → Pilih Yes

## 3: Reset semua rules

```bash
iptables -F
iptables -X
iptables -t nat -F
iptables -t nat -X

```
## 4: Set default policy
```bash
iptables -P INPUT DROP
iptables -P OUTPUT ACCEPT
iptables -P FORWARD ACCEPT

```
## 5: Izinkan koneksi terkait
```bash

iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

```
## 6: Izinkan koneksi lokal (loopback)
```bash

iptables -A INPUT -i lo -j ACCEPT

```
## 7: Izinkan port yang dibutuhkan 
sesuaikan kebutuhan ini hanya contoh
```bash

iptables -A INPUT -p tcp --dport 22 -j ACCEPT       # SSH
iptables -A INPUT -p tcp --dport 212 -j ACCEPT      # Custom port
iptables -A INPUT -p tcp --dport 51820 -j ACCEPT    # WireGuard
iptables -A INPUT -p tcp --dport 9443 -j ACCEPT     # Custom port
iptables -A INPUT -p tcp --dport 8000 -j ACCEPT     # Custom por

```
## 8: Izinkan port range 1000-1010
```bash

for port in $(seq 1000 1010); do
  iptables -A INPUT -p tcp --dport $port -j ACCEPT
done

```
## 9: Blokir ping dan ICMP batasi
```bash

iptables -A INPUT -p icmp --icmp-type echo-request -m limit --limit 1/second -j ACCEPT
iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
```
Melindungi server dari flood ICMP (ping flood) atau DDoS attack dengan membatasi hanya 1 ping per detik.
Jika lebih dari itu, ping akan di-drop, sehingga mengurangi beban dan potensi abuse

## 9:  Install iptables-persistent jika belum ada 
```bash

if ! dpkg -l | grep -q iptables-persistent; then
  echo "Installing iptables-persistent..."
  apt update && apt install -y iptables-persistent
fi

```
## 10:  Simpan aturan secara manual (kalau sudah diubah):
(ubuntu/debian)
```bash
sudo netfilter-persistent save

```
atau
```bash
sudo iptables-save > /etc/iptables/rules.v4
```

Kalau pakai CentOS/RHEL/AlmaLinux
```bash
sudo iptables-save > /etc/sysconfig/iptables

```
##11. Pastikan layanan iptables aktif:
```bash
sudo systemctl enable iptables
sudo systemctl start iptables
```

## 12: Aktifkan netfilter-persistent saat boot
```bash
systemctl enable netfilter-persistent
systemctl restart netfilter-persistent

```
atau
```bash
sudo systemctl enable iptables

```

##  ✅ Iptables rules applied & saved successfully!"
sampai dsini sudah selesai
coba cek iptables
```bash
sudo iptables -S
```
atau
```bash
sudo iptables -L -v -n

```
atau
```bash
sudo iptables -L INPUT --line-numbers

```

##  ✅ Menambah Baru ip tabless baru
ulangi langkah penambahan pot udp/tcp/icmp stelah itu save undate
```bash

iptables -A INPUT -p tcp --dport 22 -j ACCEPT       # SSH
iptables -A INPUT -p tcp --dport 212 -j ACCEPT      # Custom port
sudo netfilter-persistent save
sudo iptables-save > /etc/iptables/rules.v4
sudo iptables -S
sudo iptables -L -v -n
```

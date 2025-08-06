# ✅ Langkah Install dan Setup Fail2Ban

## 1: Install fail2ban
```bash
sudo apt update
sudo apt install fail2ban -y

```
## 2: Copy konfigurasi default ke custom
```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local

```
## 3: Edit konfigurasi

```bash
sudo nano /etc/fail2ban/jail.local


```
# 4: Cari bagian [sshd] dan ubah seperti ini (kalau perlu):
```bash
[sshd]
enabled = true
port    = ssh
filter  = sshd
logpath = /var/log/auth.log
maxretry = 5
bantime = 3600
findtime = 600


```
maxretry = 5 → gagal login 5x

bantime = 3600 → IP diblokir selama 1 jam

findtime = 600 → dihitung 5x gagal dalam 10 menit

## Restart fail2ban

```bash
sudo systemctl restart fail2ban
sudo systemctl enable fail2ban

```
## 5: Cek status fail2ban

```bash
sudo fail2ban-client status
sudo fail2ban-client status sshd
n

```

✅ Tambahan Proteksi (Opsional)
Aktifkan UFW firewall + whitelist port penting (SSH, WireGuard, dll).

Batasi koneksi SSH hanya dari IP tertentu jika VPS pribadi.

Gunakan port SSH selain 22 (security through obscurity).




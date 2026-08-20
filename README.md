# ZyaNet Billing — Backup DB (PRIVATE)

Backup harian otomatis database billing + settings. JANGAN share repo ini — isinya data pelanggan (nama, HP, alamat, tagihan).

## Isi
- `billing_db_YYYYMMDD_HHMMSS.db.gz` — snapshot konsisten database SQLite (gzip)
- `settings_YYYYMMDD_HHMMSS.json` — settings app

Git history = restore ke tanggal mana aja (ambil file dari commit yang diinginkan).

## Cara restore di server baru
```bash
# 1. Clone repo ini
git clone https://github.com/andipa13/zyanet-billing-backup.git
cd zyanet-billing-backup

# 2. Ambil backup terbaru (atau tanggal tertentu via git log)
LATEST=$(ls billing_db_*.db.gz | sort | tail -1)

# 3. Decompress ke lokasi DB app
gunzip -c "$LATEST" > /root/ali-jaya-billing/database/billing.db

# 4. Settings (opsional — sesuaikan host/URL kalau beda)
cp settings_*.json /root/ali-jaya-billing/settings.json

# 5. Start app
systemctl restart ali-jaya-billing
```

## Sumber backup lain
- Kode app: https://github.com/andipa13/billing-rtrw (public, push harian 03:00)
- Host Proxmox 10.10.10.6:/storage3tb/backups/billing/ (copy 90 hari)

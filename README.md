# TITIP! — Juragan Jastip

> **"Jastip adalah fenomena sehari-hari orang Indonesia — dari teman yang mudik ke
> Bandung sampai jastip Korea di Instagram — tapi pengelolaannya masih manual:
> screenshot harga di grup WA, hitung pakai kalkulator HP, ongkir dibagi pakai
> feeling, dan tagihan yang belum lunas ketumpuk di chat."**
>
> TITIP! merapikan semuanya: buka sesi → orang nitip → barang dibeli → ongkir +
> fee kehitung otomatis per orang → tagihan lunas → ringkasan WA tinggal
> copy-paste. Kasir + penagih untuk juragan jastip.

**Project harian "1 Hari 1 Project" — 12 November 2026.**
Jenis: web app (frontend-only, tanpa backend, tanpa login).
Repo: [junanda24/daily-titip](https://github.com/junanda24/daily-titip)

## Kenapa ide ini dipilih

Efek kejutnya ada di sudut pandang: semua orang kenal jastip, tapi tidak ada tools
kecil yang merapikan alurnya. Bukan "kalkulator ongkir" — ini produk manajemen
lengkap untuk satu profesi informal yang dijalani jutaan orang Indonesia. Fee
jastip umum 5–15% dari harga barang (atau flat per item), ongkir dibagi
proporsional terhadap nilai barang, dan pola bayar umum adalah DP dulu lalu
pelunasan — semua aturan nyata itu yang dikodekan ke dalam app (sumber:
jastipali.com, paxel.co, balancioindo.com, joinbareng.com — diakses 2026-11-12).

## Fitur

- **Beranda** — ringkasan semua sesi aktif: total omzet, uang masuk, sisa tagihan,
  plus daftar sesi dengan bilah progres pelunasan masing-masing.
- **Sesi jastip (multi-sesi)** — nama, kota tujuan, tanggal tutup PO + hitung
  mundur, ongkir total, mode fee (persen dari harga barang ATAU flat per item),
  nilai fee. Sesi bisa diarsipkan dan dihapus.
- **Katalog barang per sesi** — tambah barang (nama, harga Rp, stok opsional),
  edit harga, hapus barang. Hapus ditolak dengan pesan jelas bila barang sudah
  ada yang nitip.
- **Titipan per orang** — ketik nama orang (baru = otomatis dibuat), pilih barang,
  isi jumlah; tolak bila melebihi stok. Dikelompokkan per orang.
- **Status ganda** — status barang (Dititip → Dibeli → Diantar) dan status bayar
  (Belum bayar / DP / Lunas) per titipan.
- **Hitung otomatis** — tagihan per orang = subtotal barang + bagian ongkir
  (dibagi proporsional terhadap nilai barang) + fee jastip. Semua tagihan
  dihitung ulang seketika saat ongkir/fee diubah. DP dihitung 50% dari tagihan
  saat rekap uang masuk.
- **Bagikan ke WhatsApp** — tombol "Salin Tagihan" per orang menyalin teks rapi
  (rincian barang × jumlah = harga, ongkir, fee, total, status bayar) siap tempel
  ke WA, lengkap dengan toast konfirmasi.
- **Sesi contoh "Jastip Bandung"** — sudah terisi saat pertama dibuka (5 barang,
  3 pemesan, status beragam), bisa dihapus seperti sesi biasa.
- **Momen delight** — stempel karet "LUNAS" beranimasi + confetti kertas saat
  sebuah sesi mencapai 100% lunas; bilah progres beranimasi; bottom sheet di
  mobile; toast untuk setiap konfirmasi.
- **Data bertahan** — semua tersimpan di localStorage (`titip.v1`), utuh setelah
  reload. Routing hash `#/sesi/<id>` sehingga sesi bisa di-bookmark.

## Cara pakai

1. Buka link hasil di HP (atau buka `src/index.html` langsung di browser).
2. Sesi contoh "Jastip Bandung" sudah terisi — jelajahi dulu, atau hapus dan mulai
   dari nol.
3. Tekan **Buka Sesi Baru**: isi nama (mis. "Jastip Jogja"), kota tujuan,
   tanggal tutup PO, ongkir total, dan fee (mis. 10%).
4. Di dalam sesi, tambah barang ke katalog (nama + harga + stok bila ada).
5. Tambah titipan: ketik nama orang, pilih barang, isi jumlah.
6. Majukan status barang saat barang dibeli/diantar; ubah status bayar saat
   orang transfer DP/lunas.
7. Tekan **Salin Tagihan** di kartu seseorang → tempel ke WhatsApp orang itu.
8. Saat semua orang lunas, nikmati stempel LUNAS + confetti, lalu arsipkan
   sesinya.

## Cara menjalankan dari source

Tidak butuh build step — ini satu file HTML statis:

```bash
# paling gampang: buka langsung
open src/index.html        # macOS
xdg-open src/index.html    # Linux
# atau sajikan lewat server statis apa pun:
npx serve src
```

## Tech stack

- HTML + CSS + JavaScript murni (single-file, tanpa framework build)
- localStorage untuk persistensi (modul `store` tunggal)
- Font: Plus Jakarta Sans (Google Fonts)
- Pola kode mengikuti `ARCHITECTURE.md` (openspec-web-builder): satu objek state,
  data terpisah dari logic, fungsi render murni, handler tipis
- Desain mengikuti skill `local-impeccable` (mode Operate) + craft floor:
  dunia visual "label bagasi + struk", ikon SVG stroke (tanpa emoji sebagai ikon),
  tanpa gradient text, tanpa kicker di atas heading

## Struktur folder

```
2026-11-12-titip/
├── README.md
├── src/
│   └── index.html          # seluruh app (HTML + CSS + JS, hasil export artifact)
└── openspec/
    └── changes/
        └── archive/
            └── 2026-11-12-titip/
                ├── proposal.md
                ├── design.md
                ├── specs/titip/spec.md
                └── tasks.md
```

## Link hasil

- App (artifact): `titip-juragan-jastip` (link dibagikan saat delivery)
- Repo: https://github.com/junanda24/daily-titip
- Rilis: 2026-11-12

## Catatan / temuan saat build

- **Angka seed terverifikasi manual sampai rupiah** oleh builder: Sari Rp 177.656,
  Budi Rp 163.538, total omzet Rp 461.200, uang masuk Rp 259.425 (56%).
- **Ongkir proporsional, bukan rata-rata:** pembeli barang mahal menanggung ongkir
  lebih besar — sesuai praktik jastip nyata.
- **DP = 50% tagihan** saat rekap uang masuk (konvensi umum jastip; bukan aturan
  baku, tapi eksplisit di spec).
- **Stempel LUNAS hilang lagi bila ada titipan baru yang belum bayar** — benar
  secara logika (sesi tidak lagi 100% lunas).
- **Tidak diuji klik-per-klik:** modal konfirmasi hapus pesanan/hapus sesi memakai
  komponen modal yang sama persis dengan arsip (sudah diuji); sheet tambah/ubah
  barang memakai sistem formulir yang sama dengan sheet sesi & pesanan (sudah
  diuji penuh). Mode gelap terdefinisi via `prefers-color-scheme` tapi audit
  hanya menangkap bingkai terang.
- **Sengaja tidak dibuat:** tanpa login/sinkronisasi antar perangkat; tanpa
  integrasi kurir/cek ongkir otomatis (ongkir diinput manual); tanpa pembayaran
  dalam app (bayar tetap via transfer/QRIS di luar app).

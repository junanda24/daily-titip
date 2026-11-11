# Proposal — TITIP! Juragan Jastip (2026-11-12)

## Apa ini
Web app mobile-first untuk mengelola bisnis **jastip (jasa titip beli)** — dari buka sesi jastip,
katalog barang, titipan per orang, sampai hitungan otomatis per orang (harga barang + ongkir
+ fee jastip) dan status bayar. Semua data tersimpan di localStorage (frontend-only, tanpa
login, tanpa backend).

## Kenapa ide ini mengejutkan
Jastip adalah fenomena sehari-hari orang Indonesia — dari teman yang mudik ke Bandung
sampai jastip Korea di Instagram — tapi pengelolaannya masih manual: screenshot harga di
grup WA, hitung pakai kalkulator HP, ongkir dibagi pakai feeling, dan tagihan yang belum
lunas ketumpuk di chat. Tidak ada satu pun tools kecil yang merapikan alur ini. TITIP!
mengambil dunia itu dan menjadikannya produk beneran: buka sesi → orang nitip → barang
dibeli → ongkir + fee kehitung otomatis per orang → tagihan lunas → ringkasan WA tinggal
copy-paste. Sudut pandangnya bukan "kalkulator ongkir", tapi **"kasir + kasir + penagih
untuk juragan jastip"**.

## Kenapa bentuk output ini (web app, frontend-only)
- Jastip dikelola dari HP, di tengah chat WA — web app mobile-first adalah tempatnya.
- Tidak butuh backend: semua data milik satu orang (si juragan jastip) di perangkatnya
  sendiri; localStorage cukup dan tanpa login.
- Bisa dibagikan sebagai link publik (static artifact) — calon pengguna lain bisa langsung
  coba tanpa install.

## Kenapa bukan game / npm / Flutter hari ini
- Kemarin (11 Nov) game OJOL!; 10 Nov web app KOSTKU!. Rotasi hari ini web app lagi
  karena ide jastip ini kuat sebagai produk dan belum ada yang sejenis di BUILT.md.
- Bukan npm: hitung jastip butuh UI dan alur, bukan sekadar fungsi library.

## Uji bar scope
- Bukan utilitas satu fungsi: 5+ fitur saling nyambung (sesi, katalog, titipan, hitung
  otomatis, status bayar, arsip, share WA), state bertahan, alur end-to-end.
- Selevel patokan: ArisanKu / KondanganKu / KOSTKU! — produk manajemen nyata, bukan
  kalkulator.

## Riset yang mendasari hitungan
- Fee jastip umum 5–15% dari harga barang (Paxel: 10–30%; contoh jastip Korea 8–10%),
  atau tarif flat per item. → Default fee 10%, bisa diubah per sesi (% atau flat).
- Ongkir internasional/domestik sering dibagi ke pembeli; praktik umum: dibagi
  proporsional terhadap nilai barang. → Ongkir total sesi dibagi proporsional nilai
  barang tiap orang (bukan rata-rata buta).
- Pola bayar umum: DP dulu, pelunasan setelah barang sampai. → Status bayar tiga
  tingkat: Belum bayar / DP / Lunas.
- Sumber: jastipali.com, paxel.co, balancioindo.com, joinbareng.com (diakses 2026-11-12).

## Fitur inti
1. Sesi jastip (multi-sesi): nama, kota tujuan, tanggal tutup PO + hitung mundur, ongkir
   total, mode fee (% / flat per item), arsip & hapus sesi.
2. Katalog barang per sesi: nama, harga, stok (opsional).
3. Titipan: pilih orang (nama bebas), pilih barang, jumlah; satu orang bisa nitip
   banyak barang.
4. Hitung otomatis per orang: subtotal barang + bagian ongkir (proporsional nilai) +
   fee jastip; total sesi, uang masuk, sisa tagihan.
5. Status ganda per titipan: status barang (Dititip → Dibeli → Diantar) dan status
   bayar (Belum / DP / Lunas).
6. Dashboard: total sesi aktif, omzet, uang masuk, tagihan belum lunas.
7. Bagikan: teks ringkasan tagihan per orang format WA (copy ke clipboard).
8. Sesi contoh ("Jastip Bandung") terisi data realistis saat pertama dibuka, bisa
   dihapus.
9. Momen delight: stempel "LUNAS" + confetti saat satu sesi 100% lunas; bilah progres
   sesi beranimasi.

## Bukan dibuat (sengaja)
- Tanpa login/sinkronisasi antar perangkat (localStorage saja).
- Tanpa integrasi kurir nyata / cek ongkir otomatis (ongkir diinput manual per sesi).
- Tanpa pembayaran dalam app (bayar tetap via transfer/QRIS di luar app).

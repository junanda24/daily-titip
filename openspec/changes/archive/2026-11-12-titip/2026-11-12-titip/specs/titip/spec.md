# Spec — TITIP! Juragan Jastip

## Sesi jastip

- **SHALL-01** Pengguna dapat membuat sesi jastip dengan: nama sesi, kota tujuan,
  tanggal tutup PO, ongkir total (Rp, boleh 0), mode fee (persen dari harga barang
  ATAU flat per item), dan nilai fee.
  - WHEN pengguna mengisi form sesi dan menekan "Buka Sesi" THEN sesi baru muncul di
    daftar dan menjadi sesi aktif.
  - WHEN tanggal tutup PO sudah lewat THEN sesi menampilkan label "PO ditutup" dan
    hitung mundur menampilkan "Tutup".
- **SHALL-02** Pengguna dapat melihat daftar semua sesi (aktif + arsip), membuka satu
  sesi, mengarsipkan sesi selesai, dan menghapus sesi (dengan konfirmasi).
  - WHEN sesi diarsipkan THEN sesi pindah ke bagian Arsip dan tidak masuk hitungan
    dashboard aktif.
- **SHALL-03** Sesi contoh "Jastip Bandung" sudah terisi saat pertama kali dibuka,
  berisi minimal 4 barang dan 3 orang pennitip dengan status beragam, dan dapat
  dihapus seperti sesi biasa.

## Katalog barang

- **SHALL-04** Di dalam sesi, pengguna dapat menambah barang (nama, harga Rp, stok
  opsional), mengedit harga, dan menghapus barang yang belum pernah dititip.
  - WHEN barang dihapus padahal sudah ada titipannya THEN hapus ditolak dengan pesan
    yang menjelaskan alasannya.

## Titipan

- **SHALL-05** Pengguna dapat menambah titipan: pilih/ketik nama orang, pilih barang
  dari katalog, isi jumlah.
  - WHEN nama orang baru diketik THEN orang itu dibuat otomatis dan muncul di daftar
    penagihan.
  - WHEN jumlah melebihi stok barang (bila stok diisi) THEN titipan ditolak dengan
    pesan stok tidak cukup.
- **SHALL-06** Setiap titipan punya status barang (Dititip → Dibeli → Diantar) yang
  bisa dimajukan selangkah demi selangkah, dan status bayar (Belum bayar / DP /
  Lunas) yang bisa diubah.
  - WHEN semua titipan satu orang berstatus Lunas THEN orang itu ditandai lunas.
- **SHALL-07** Pengguna dapat menghapus satu titipan (dengan konfirmasi).

## Hitung otomatis

- **SHALL-08** Tagihan per orang = subtotal barang (Σ harga × jumlah) + bagian ongkir
  + fee jastip.
  - Ongkir total sesi dibagi proporsional terhadap nilai barang tiap orang
    (bagian = ongkir × nilai orang / nilai semua barang). Bila nilai semua barang 0,
    bagian ongkir 0.
  - Fee persen = persen × subtotal barang orang itu; fee flat = flat × total jumlah
    item orang itu.
  - WHEN ongkir atau fee sesi diubah THEN semua tagihan dihitung ulang seketika.
- **SHALL-09** Dashboard sesi menampilkan: total nilai barang, total fee, total
  ongkir, total tagihan semua orang, uang masuk (jumlah tagihan orang berstatus
  Lunas + setengah tagihan orang berstatus DP — DP dihitung 50% dari tagihan), dan
  sisa tagihan.
- **SHALL-10** Semua angka Rupiah diformat Indonesia (Rp 12.500, pemisah ribuan titik,
  tanpa desimal).

## Dashboard beranda

- **SHALL-11** Beranda menampilkan ringkasan semua sesi aktif: jumlah sesi aktif,
  total omzet (total tagihan semua sesi aktif), uang masuk, sisa tagihan, dan daftar
  sesi dengan progres pelunasan masing-masing.

## Bagikan (WhatsApp)

- **SHALL-12** Pengguna dapat menyalin teks tagihan per orang dalam format rapi
  (nama sesi, rincian barang × jumlah = harga, ongkir, fee, total, status bayar)
  ke clipboard dengan satu ketukan, siap ditempel ke WhatsApp.
  - WHEN disalin THEN muncul toast konfirmasi "Ringkasan disalin".

## Penyimpanan & keadaan

- **SHALL-13** Semua data (sesi, barang, titipan, status) tersimpan di localStorage
  dan utuh setelah reload.
- **SHALL-14** Empty state yang jelas: belum ada sesi; sesi tanpa barang; sesi tanpa
  titipan — masing-masing dengan ajakan bertindak.
- **SHALL-15** Tidak ada tombol mati, teks placeholder, atau error console pada alur
  utama (buat sesi → tambah barang → tambah titipan → ubah status → salin ringkasan).

## Delight

- **SHALL-16** Saat sebuah sesi mencapai 100% lunas (semua orang Lunas), tampil
  stempel "LUNAS" beranimasi + confetti sederhana, sekali per pencapaian.
- **SHALL-17** Bilah progres pelunasan sesi beranimasi saat nilai berubah.

# Design — TITIP! Juragan Jastip

## Stack
- Web artifact static (single-file, client-side only): `index.html` + `styles.css` +
  `app.js` mengikuti pola ARCHITECTURE.md §1 (satu objek state, DATA terpisah dari
  logic, fungsi render murni, handler tipis, satu modul `store` untuk localStorage,
  IIFE/module scope).
- Tanpa backend (alasan di proposal.md). Tanpa framework build — artifact static.
- Font: Plus Jakarta Sans (Google Fonts) — wajah Indonesia yang ramah, display +
  body satu keluarga.

## Dunia visual (impeccable: mode Operate)
- Suasana: **kios titipan di pasar + label bagasi travel**. Latar kertas hangat
  (#FBF6EC), tinta pekat (#221D16), aksen oranye jastip (#E85D04 / #EA580C) untuk
  aksi, hijau teal (#0F766E) untuk uang masuk/lunas, merah bata (#B42318) untuk
  tagihan.
- Elemen khas: "label bagasi" pada header sesi (garis putus-putus + lubang),
  stempel karet "LUNAS" (border tebal, rotate -8deg), kartu tagihan seperti struk.
- Anti-AI-slop: tanpa kartu ikon-seragam sebagai struktur halaman; tanpa kicker di
  atas heading; tanpa gradient text; tanpa emoji sebagai ikon — ikon digambar sebagai
  SVG stroke konsisten (tas, koin, truk, centang, salin, tambah, hapus).
- Tipografi: display 28–34px bold untuk nama sesi, angka Rupiah tabular-nums,
  hierarki berat yang jelas.

## Struktur layar (mobile-first, max-width 480px, satu kolom)
1. **Beranda**: sapaan + ringkasan 3 angka (omzet, uang masuk, sisa tagih) sebagai
   baris angka besar dengan label — bukan kartu-kartu seragam; daftar sesi dengan
   bilah progres; tombol "Buka Sesi Baru".
2. **Detail sesi**: header label-bagasi (nama, kota, hitung mundur tutup PO),
   ringkasan keuangan sesi (struk), tab/section: Barang (katalog + tambah),
   Titipan (per orang, dikelompokkan, dengan stepper status + tombol salin WA),
   Pengaturan sesi (ongkir, fee, arsip, hapus).
3. **Sheet/modal**: form tambah sesi, tambah barang, tambah titipan — bottom sheet
   di mobile, bukan modal tengah yang mengganggu.
4. **Toast** untuk konfirmasi (disalin, tersimpan, ditolak).

## State — lima keranjang (ARCHITECTURE.md §2)
1. **Lokal**: tab aktif di detail sesi, sheet yang terbuka, draft form.
2. **URL**: sesi yang dibuka via hash `#/sesi/<id>` (bisa di-bookmark/dibagikan).
3. **Server state**: tidak ada (tanpa backend).
4. **Global client UI**: toast.
5. **Form**: input terkontrol biasa (tanpa library).

## Data model (localStorage key `titip.v1`)
```js
{ sessions: [ {
  id, name, city, closeDate, shippingFee, feeMode: 'percent'|'flat', feeValue,
  archived: false,
  items: [ { id, name, price, stock|null } ],
  orders: [ { id, person, itemId, qty, itemStatus: 'dititip'|'dibeli'|'diantar',
              payStatus: 'belum'|'dp'|'lunas' } ]
} ] }
```
- Semua hitungan adalah fungsi murni dari data di atas (`calcPersonBill`,
  `calcSessionTotals`) — tidak ada angka yang disimpan ganda.
- DP = 50% dari tagihan orang itu, dihitung saat rekap uang masuk.

## Animasi & delight
- Satu momen authored: stempel LUNAS (scale dari 1.6 → 1 dengan ease-out + sedikit
  rotate) + confetti potongan kertas (canvas/partikel CSS, sekali per pencapaian).
- Bilah progres: transisi lebar 600ms ease-out.
- Sheet naik dari bawah (translateY + fade), toast slide-up.
- Hormati `prefers-reduced-motion`.

## Verifikasi
- Checklist SHALL-01..17 di spec, satu per satu, lewat alur klik nyata di artifact.
- Cek ARCHITECTURE.md §1: tidak ada logic bisnis di handler, querySelector di-cache,
  localStorage hanya lewat `store`.

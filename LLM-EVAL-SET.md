# PBIT-S — Eval Set untuk LLM (v1.0)

Kumpulan kasus untuk menguji perbaikan review: pelestarian makna, penandaan berlebih, keterbacaan, kepatuhan format.
Jalankan dalam Mode C (audit JSON) kecuali dinyatakan lain. Nilai: makna benar, pelanggaran tepat, format valid.

## Cara nilai

- **Makna (bobot 40):** tanpa angka/objek/syarat/urutan/kepastian baru. Tandai `[INFO-KURANG]` bila sumber kurang, jangan tebak.
- **Ketepatan aturan (bobot 30):** ID aturan dan severity benar; tanpa false positive pada kata fungsi baku atau imbuhan normal.
- **Keterbacaan (bobot 20):** hasil ≤20 (prosedur) / ≤25 (deskripsi) kata, aktif untuk prosedur, syarat di depan + koma.
- **Format (bobot 10):** JSON valid sesuai LLM-CHECKLIST.md §3 (termasuk `classification`).

## Kasus

### E01 — Jangan tebak objek (makna)
Input: "Baut harus dikencangkan sebelum penutupnya dilepas jgn lupa cek."
Lulus bila: `rewritten_id` tanpa "tekanan" atau objek baru; ada `[INFO-KURANG]` bila objek tak disebut; PBIT-3.6, PBIT-1.14, PBIT-5.4 ditandai; GR-ID-3 warning tanpa tebakan.

### E02 — Jangan tambah angka (makna)
Input: "PERHATIAN: Jangan merusak ulir saat memasang saringan."
Lulus bila: tidak ada "20 Nm" atau nilai baru dalam perbaikan; label tetap PERHATIAN (benda).

### E03 — Kata fungsi bukan pelanggaran (false positive)
Input: "Sebelum Anda melepas penutup, matikan mesin. Pastikan bahwa tidak ada kebocoran."
Lulus bila: `tidak, bahwa, Anda` tidak ditandai sebagai PBIT-1.1; verdict LOLOS atau hanya warning gaya yang valid.

### E04 — Imbuhan normal bukan pelanggaran (false positive)
Input: "Teknisi memasang penutup. Pompa memasok bahan bakar."
Lulus bila: `memasang/memasok` tidak ditandai PBIT-3.5; kalimat aktif dikenali benar.

### E05 — Inisialisasi spesifik dipertahankan (makna vs konsistensi)
Input: "Inisialisasi basis data sebelum Anda memulai layanan."
Lulus bila: tidak diganti menjadi "Mulai basis data"; saran berupa `[ISTILAH-BARU: inisialisasi basis data — sistem/komponen — penyiapan keadaan awal]` atau mempertahankan istilah dengan dasar.

### E06 — Nonaktifkan vs matikan lintas konteks (makna)
Input: "Nonaktifkan akun sebelum Anda mematikan server."
Lulus bila: keduanya dipertahankan (konteks berbeda); tidak diseragamkan paksa menjadi "Matikan akun".

### E07 — Klasifikasi berdasar bukti (prosedur vs deskripsi)
Input: "Pompa memasok bahan bakar ke mesin."
Lulus bila: `type=deskripsi` (pernyataan, bukan perintah); tidak diubah menjadi "Pasang..." atau imperatif; batas 25 kata dipakai.

### E08 — Catatan vs langkah (struktur)
Input: "Langkah 3. Kencangkan baut hingga 20 Nm. Catatan: Nilai 20 Nm berlaku untuk suhu 20 °C. Catatan: Jangan lanjut sebelum tekanan normal."
Lulus bila: kalimat catatan pertama (keberlakuan dari sumber) lolos; kalimat kedua ditandai PBIT-5.5 dan dipindah ke langkah.

### E09 — Ejaan bentuk terikat (EYD)
Input: "Pasang lapisan anti-korosi. Non-aktifkan panel."
Lulus bila: perbaikan `antikorosi` (serangkai) dan `nonaktifkan` (serangkai); aturan PBIT-8.2/PBIT-1.14.

### E10 — Format audit lengkap
Input: E01 di atas.
Lulus bila: JSON valid, ada `classification` dengan `basis`, ada `counts` dengan hitung kurung/angka per Bab 8, `rewritten_id` tanpa fakta baru.

## Target lulus awal

- Makna: 100% (E01, E02, E05, E06 tanpa fakta baru).
- False positive: 0 pada E03–E04.
- Format: JSON valid pada semua kasus Mode C.

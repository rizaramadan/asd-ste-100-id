# PBIT-S — Checklist dan Output JSON untuk LLM (v1.0)

## 1. Cara pakai (3 mode)

- **Tulis baru:** baca LLM-SYSTEM-PROMPT.md → tulis → cek diri dengan rubrik §2 → keluarkan JSON §3.
- **Periksa teks:** klasifikasikan tiap kalimat (prosedur/deskripsi/keselamatan/catatan) → hitung kata Bab 8 → cek kamus → keluarkan JSON.
- **RAG:** ambil hanya aturan yang dilanggar dari LLM-RULES.json (jangan tempel 53 sekaligus). Prioritas error dulu.

## 2. Rubrik nilai (lolos/gagal)

GAGAL RILIS jika ada 1 error:
PBIT-1.1,1.2,1.3,1.6,1.7,1.11,1.13,1.14,3.1,3.2,3.4,3.5,3.6,4.1,4.2,4.5,5.1,5.2,5.3,5.4,5.5,6.3,6.6,7.1,7.2,8.1,9.2,9.3.

PERBAIKI bila mungkin (warning):
1.4,1.8,1.10,1.12,2.1,2.2,3.3,3.7,4.3,4.4,6.1,6.2,6.4,6.5,7.3,8.2,8.3,8.4,9.1,9.4,GR-ID-1–4.

INFO hitung kata: 1.5,1.9,8.5,8.6,8.7.

Cek cepat 60 detik:
1. Klasifikasi benar? Prosedur≤20, deskripsi/catatan≤25, paragraf≤6?
2. Aktif? Prosedur tanpa di-/ter- pasif? Tanpa sedang/telah-sedang-akan berlapis? Tanpa melakukan peng-?
3. Kamus? Tanpa dgn/tdk/yg/save/print/swap? Sinonim konsisten? Ejaan KBBI?
4. Frasa≤3 kata? Syarat di depan+koma? Daftar vertikal untuk 4+ aksi?
5. Keselamatan: label benar, perintah di depan, akibat spesifik?

## 3. Skema output (wajib JSON valid, tanpa teks luar)

```json
{
  "verdict": "LOLOS | GAGAL | PERLU_PERBAIKAN",
  "summary_id": "1 kalimat Indonesia, maks 25 kata.",
  "counts": {"kalimat": 0, "kata_maks_prosedur": 0, "kata_maks_deskripsi": 0, "kalimat_per_paragraf_maks": 0},
  "violations": [
    {
      "rule": "PBIT-5.1",
      "severity": "error",
      "sentence": "kalimat asli persis",
      "why_id": "alasan 1 kalimat",
      "fix_id": "perbaikan minimal persis"
    }
  ],
  "rewritten_id": "teks penuh yang sudah diperbaiki, dengan label PERINGATAN/PERHATIAN/Catatan bila perlu"
}
```

Aturan output:
- `verdict=GAGAL` jika ada severity error. `PERLU_PERBAIKAN` jika hanya warning. `LOLOS` jika nol pelanggaran.
- `counts` dihitung dengan: kurung=1 kata, isi kurung dinilai sendiri, angka/satuan/singkatan/ID/kutipan/nama=1 kata, berhubung=1 kata, nomor langkah tidak dihitung.
- `rewritten_id` harus lolos semua error, pertahankan istilah resmi, jangan tambah info baru.
- Jika input sudah lolos, `violations=[]`, `rewritten_id` = input (normalisasi spasi saja).

## 4. Contoh mini

Input: "Baut harus dikencangkan sebelum penutupnya dilepas jgn lupa cek."

Output:
```json
{
  "verdict": "GAGAL",
  "summary_id": "Teks gagal karena pasif, nya kabur, singkatan, dan urutan syarat salah.",
  "counts": {"kalimat": 1, "kata_maks_prosedur": 9, "kata_maks_deskripsi": 0, "kalimat_per_paragraf_maks": 1},
  "violations": [
    {"rule": "PBIT-3.6", "severity": "error", "sentence": "Baut harus dikencangkan sebelum penutupnya dilepas jgn lupa cek.", "why_id": "Prosedur memakai pasif tanpa imperatif.", "fix_id": "Kencangkan baut."},
    {"rule": "PBIT-1.14", "severity": "error", "sentence": "Baut harus dikencangkan sebelum penutupnya dilepas jgn lupa cek.", "why_id": "Singkatan jgn tidak baku.", "fix_id": "Jangan."},
    {"rule": "GR-ID-3", "severity": "warning", "sentence": "Baut harus dikencangkan sebelum penutupnya dilepas jgn lupa cek.", "why_id": "nya kabur dan cek tanpa objek.", "fix_id": "Lepas penutup. Periksa tekanan."},
    {"rule": "PBIT-5.4", "severity": "error", "sentence": "Baut harus dikencangkan sebelum penutupnya dilepas jgn lupa cek.", "why_id": "Syarat di belakang tanpa koma.", "fix_id": "Sebelum Anda melepas penutup, kencangkan baut."}
  ],
  "rewritten_id": "Sebelum Anda melepas penutup, kencangkan baut. Lepas penutup. Periksa tekanan."
}
```

## 5. Batas aman LLM

- Jangan cipta istilah/singkatan baru. Jika butuh istilah, tandai `[ISTILAH-BARU: nama — kategori — definisi]` di `why_id`.
- Jangan ubah angka/satuan/nilai. Jangan pindah PERINGATAN jadi Catatan.
- Jika pelaku deskripsi tak diketahui, boleh satu pasif dan tulis ketidaktahuan eksplisit.
- Jika ragu klasifikasi prosedur vs deskripsi, anggap prosedur (batas 20 kata, wajib imperatif).

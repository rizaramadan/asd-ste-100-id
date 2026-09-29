# ASD-STE100-ID — Adaptasi ASD-STE100 untuk Bahasa Indonesia

Adaptasi penuh prinsip **ASD-STE100 Simplified Technical English (Issue 9, Januari 2025)**
untuk penulisan teknis dalam **Bahasa Indonesia** yang jelas, aman, dan mudah diterjemahkan.

> Bukan terjemahan resmi ASD-STE100. Hak cipta ASD-STE100 milik ASD
> (Aerospace, Security and Defence Industries Association of Europe).
> Dokumen ini menafsir ulang seluruh prinsipnya untuk Bahasa Indonesia.

**Prinsip 0: jangan ubah makna.** Angka, satuan, objek, syarat, urutan, dan kepastian tidak boleh ditebak. Lihat `LLM-EVAL-SET.md` untuk uji makna.

## Isi

### Versi lengkap (untuk manusia)

- **[`PEDOMAN-PBIT-S.md`](./PEDOMAN-PBIT-S.md)** — Pedoman Bahasa Indonesia Teknis Sederhana (PBIT-S).
  Mencakup 53 aturan dalam 9 seksi + 4 rekomendasi, kamus sebagai panduan istilah pilihan,
  22 kategori nomina teknis, 4 kategori verba teknis, contoh sebelum–sesudah, daftar periksa, dan template.

### Versi LLM (untuk konteks model)

| File | Fungsi | Ukuran |
|---|---|---|
| [`LLM-SYSTEM-PROMPT.md`](./LLM-SYSTEM-PROMPT.md) | System prompt v1.1: prioritas makna, 3 mode (tulis/sunting/audit) | ~4 KB |
| [`LLM-RULES.json`](./LLM-RULES.json) | 57 entri mesin (53 aturan + 4 GR): id, severity, rule, check, fix, contoh | ~30 KB |
| [`LLM-DICTIONARY.csv`](./LLM-DICTIONARY.csv) | Kamus awal ~106 baris (panduan pilihan, bukan daftar izin mutlak) | ~9 KB |
| [`LLM-CHECKLIST.md`](./LLM-CHECKLIST.md) | Rubrik LOLOS/GAGAL + skema output JSON Mode C + contoh tanpa fakta baru | ~5 KB |
| [`LLM-EVAL-SET.md`](./LLM-EVAL-SET.md) | 10 kasus uji: makna, false positive, keterbacaan, format | ~4 KB |

## Cara pakai versi LLM

1. **Tulis (Mode A):** gunakan [`LLM-SYSTEM-PROMPT.md`](./LLM-SYSTEM-PROMPT.md) saja.
2. **Sunting (Mode B):** prompt + [`LLM-DICTIONARY.csv`](./LLM-DICTIONARY.csv); keluarkan teks + daftar perubahan.
3. **Audit (Mode C):** prompt + kamus + [`LLM-CHECKLIST.md`](./LLM-CHECKLIST.md); keluarkan HANYA JSON.
4. **RAG:** ambil per `id` dari [`LLM-RULES.json`](./LLM-RULES.json) hanya aturan yang dilanggar.
5. **Uji:** jalankan [`LLM-EVAL-SET.md`](./LLM-EVAL-SET.md) (target: makna 100%, false positive 0, JSON valid).
6. **Rujukan penuh:** [`PEDOMAN-PBIT-S.md`](./PEDOMAN-PBIT-S.md).

Catatan kamus: kata fungsi baku (tidak, yang, untuk, pada, dalam, bahwa, jangan, dan) sah per KBBI/EYD walau tidak tercantum satu per satu. Penetapan kelas kata (mis. `uji` hanya nomina) adalah keputusan gaya proyek, bukan tata bahasa umum. Penggantian sinonim hanya bila sama makna dalam konteks (`inisialisasi` penyiapan awal ≠ `mulai`; `nonaktifkan akun` ≠ `matikan mesin`).

## Sumber acuan

- ASD-STE100 Issue 9, Standard for Technical Documentation (15 Jan 2025) — https://www.asd-ste100.org/
- FAQ resmi STE — https://asd-ste100.org/STE_faq.html
- Ringkasan 53 aturan — https://axiom-verity.com/ste/rules
- EYD V — https://ejaan.kemendikdasmen.go.id/eyd/ (tanda hubung, kata turunan)
- ISO 1087-1:2019 (acuan terminologi Issue 9)

## Riwayat

- v1.1: prioritas pelestarian makna, kamus sebagai panduan (bukan izin mutlak), 3 mode, contoh tanpa fakta rekaan, koreksi EYD (`antikorosi`, `nonaktif`), klarifikasi `sudah/telah` sebagai penanda aspek, penentu seperlunya, catatan vs langkah, set uji 10 kasus.
- v1.0: rilis awal pedoman + paket LLM.

## Lisensi

Konten adaptasi ini bebas dipakai untuk dokumentasi internal dan publik dengan atribusi.
Untuk teks resmi ASD-STE100, minta salinan resmi gratis di situs ASD-STE100.

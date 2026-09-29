# ASD-STE100-ID — Adaptasi ASD-STE100 untuk Bahasa Indonesia

Adaptasi penuh prinsip **ASD-STE100 Simplified Technical English (Issue 9, Januari 2025)**
untuk penulisan teknis dalam **Bahasa Indonesia** yang jelas, aman, dan mudah diterjemahkan.

> Bukan terjemahan resmi ASD-STE100. Hak cipta ASD-STE100 milik ASD
> (Aerospace, Security and Defence Industries Association of Europe).
> Dokumen ini menafsir ulang seluruh prinsipnya untuk Bahasa Indonesia.

## Isi

### Versi lengkap (untuk manusia)

- **`PEDOMAN-PBIT-S.md`** — Pedoman Bahasa Indonesia Teknis Sederhana (PBIT-S).
  Mencakup seluruh 53 aturan dalam 9 seksi, kamus terkendali, 22 kategori nomina teknis,
  4 kategori verba teknis, contoh sebelum–sesudah, daftar periksa, dan template.

### Versi LLM (untuk konteks model)

| File | Fungsi | Ukuran |
|---|---|---|
| `LLM-SYSTEM-PROMPT.md` | Tempel sebagai system prompt (~450 kata) | ~3 KB |
| `LLM-RULES.json` | 57 entri mesin (53 aturan + 4 GR): id, severity, rule, check, fix, contoh | ~29 KB |
| `LLM-DICTIONARY.csv` | 96 baris kamus (49 disetujui, 47 tidak): kata, kelas, makna, pengganti | ~8 KB |
| `LLM-CHECKLIST.md` | Rubrik LOLOS/GAGAL + skema output JSON wajib untuk validator LLM | ~4 KB |

## Cara pakai versi LLM

1. **Tulis/periksa:** gunakan `LLM-SYSTEM-PROMPT.md` + `LLM-DICTIONARY.csv` + `LLM-CHECKLIST.md`.
2. **RAG:** ambil per `id` dari `LLM-RULES.json` hanya aturan yang dilanggar — jangan tempel semua sekaligus.
3. **Rujukan penuh:** `PEDOMAN-PBIT-S.md` (±8.884 kata, ~15.800 token).

## Sumber acuan

- ASD-STE100 Issue 9, Standard for Technical Documentation (15 Jan 2025) — https://www.asd-ste100.org/
- Ringkasan 53 aturan — https://axiom-verity.com/ste/rules
- KBBI Daring + EYD V (acuan ejaan Indonesia)
- ISO 1087-1:2019 (acuan terminologi Issue 9)

## Lisensi

Konten adaptasi ini bebas dipakai untuk dokumentasi internal dan publik dengan atribusi.
Untuk teks resmi ASD-STE100, minta salinan resmi gratis di situs ASD-STE100.

# Kantor AI — by Agung

Kantor pixel virtual. 6 agent AI kerja tiap malam 00:30 WIB lewat GitHub Actions, hasilnya nunggu dicek di halaman kantor. **Agent tidak posting sendiri.**

| Divisi | Agent | Kerjaannya |
|---|---|---|
| Shorts & Editor | Nova | Riset tren Shorts dari judul berita RSS → 3 ide (label `berita` / `lab`) |
| | Ale | Naskah Shorts 35–45 detik, narasi Inggris, hook 2 detik |
| | Rara | Instruksi edit per shot: visual, animasi teks, transisi, SFX |
| Affiliate & Sosmed | Sari | Riset produk gadget dari RSS → 3 ide konten tips gadget |
| | Dimas | Caption TikTok / Threads / Facebook + hashtag |
| | Bima | Antrean posting 3 hari, jam WIB, status "nunggu dicek" |

Aturan yang sudah dipasang di prompt agent: tiap teks di video wajib ada animasi, dan tulisan kecil **"by Agung"** di atas frame. Tanpa watermark lain.

## Pasang (10 menit, gratis)

1. **Bikin repo** GitHub baru, nama `kantor-ai`, **Public** (Pages gratis butuh public), lalu upload semua isi folder ini.
2. **API key Gemini**: ambil di https://aistudio.google.com/apikey (gratis, tanpa kartu).
3. **Simpan key**: repo → Settings → Secrets and variables → Actions → New repository secret
   - Name: `GEMINI_API_KEY`
   - Secret: tempel key-nya
4. **Nyalakan Pages**: Settings → Pages → Source: *Deploy from a branch* → Branch `main`, folder `/ (root)` → Save.
   Alamat kantor: `https://<username>.github.io/kantor-ai/`
5. **Tes sekali**: tab Actions → "Shift malam Kantor AI" → Run workflow. Tunggu ±2 menit, buka alamat kantor.

Habis itu jalan sendiri tiap malam.

## Isi

```
index.html              halaman kantor pixel (buka di HP juga enak)
agents/office.py        satu shift: 6 agent, Python stdlib doang
data/shift.json         hasil shift terakhir (ditulis robot)
data/history.json       7 shift terakhir
.github/workflows/      jadwal cron 17:30 UTC = 00:30 WIB
```

## Catatan teknis

- **Model**: script nanya sendiri ke API model gratis apa yang aktif (`models?key=`), jadi tidak mati kalau Google ganti nama model. Mau kunci satu model: repo → Settings → Variables → `GEMINI_MODEL`.
- **Rate limit**: free tier Gemini ketat (sekitar 5 request/menit, kuota harian kecil). Satu shift = 6 request + retry otomatis kalau kena 429. Jangan dijalanin bolak-balik dalam semenit.
- **Data riset**: judul dari Google News RSS, bukan analytics TikTok/YouTube. Ide berlabel `lab` = karangan model, bukan tren nyata.
- **Gagal?** Buka Actions → log step "Agent kerja". Kalau statusnya gagal, halaman kantor tetap kebuka dan nampilin pesan errornya.

## Batasan jujur

- Skor/ide di sini **bukan** prediksi views, viral, atau penghasilan.
- Agent tidak punya akses ke akun sosmed. Antrean Bima diposting manual.
- Animasi orang jalan ke meja cuma visual, bukan tanda agent "sedang berpikir".

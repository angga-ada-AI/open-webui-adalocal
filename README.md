# Open WebUI — adaLOCAL

Resep pemasangan **Open WebUI** untuk [adaLOCAL](https://adacode.ai) di adaCODE App: dipasang dan dijalankan
di komputer pengguna dengan satu klik, langsung tersambung ke model AI adaCODE.

Antarmuka chat untuk model AI — mirip ChatGPT, jalan di komputer sendiri, sudah tersambung ke model adaCODE.

- App asli: https://github.com/open-webui/open-webui
- Lisensi app asli: BSD-3-Clause (repo ini hanya berisi resep pemasangan, bukan kode app-nya)

## Isi repo

| Berkas | Fungsi |
|---|---|
| `adacode.json` | Manifes: prasyarat, langkah `pasang`, cara `jalan`, `perbarui`. Data JSON, bukan skrip. |
| ikon & `gambar.jpg` | Ikon dan gambar kartu di katalog adaLOCAL. |

## Cara app ini masuk ke katalog adaLOCAL

Admin adaCODE mendaftarkan repo ini di `/admin/app-katalog` beserta **commit SHA lengkap**. Server mengambil
`adacode.json` pada commit itu, memeriksanya, lalu menyalinnya. Karena yang dipakai selalu commit tertentu
(bukan cabang), perubahan di repo ini **tidak** sampai ke pengguna sampai admin meninjau dan menyimpan SHA baru.

Aturan manifes:
- Aksi yang dikenal pemasang: `log`, `tulis`, `folder`, `venv`, `shell`.
- Jangan menulis rahasia di sini. Kunci API adaCODE dan alamat gateway diisi server lewat pengisi tempat
  `{{kunci_api_pelanggan}}` dan `{{gateway_adacode}}`; jalur lokal lewat `{{folder_app}}`.
- Semua berkas app wajib berada di foldernya sendiri (supaya bisa dipindah ke VPS).

## Model AI yang dipakai

| Fitur Open WebUI | Sumber | Keterangan |
|---|---|---|
| Chat | model adaCODE | `OPENAI_API_BASE_URL` = gateway adaCODE, kunci pelanggan diisi server |
| Pembuatan gambar | adaIMAGE (`adaimage-2.1-flash`) | sejak 19 Okt 2026, 1024×1024 |
| Embedding dokumen (RAG) | model lokal `sentence-transformers` | gateway adaCODE belum punya `/v1/embeddings` |
| Suara ke teks | model lokal (Whisper) | gateway adaCODE belum punya `/v1/audio/transcriptions` |

Open WebUI menyimpan setelan ke `data/webui.db` saat pertama jalan, dan nilai di sana mengalahkan variabel
lingkungan. Untuk pemasangan lama, tombol **Perbarui** menjalankan `adacode_setelan.py`, yang mengembalikan
setelan gambar ke adaIMAGE **hanya bila** setelan itu masih bawaan. Setelan yang diatur pengguna tidak disentuh.

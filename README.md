# AI ChatBot Menggunakan Data di Supabase (RAG AI Agent)

Repositori ini berisi workflow otomasi **n8n** untuk implementasi **AI Agent Chatbot berbasis RAG (Retrieval-Augmented Generation)** menggunakan database vektor **Supabase**, model bahasa & embeddings **Google Gemini**, dan integrasi dokumen PDF dari **Google Drive**.

Workflow ini difokuskan sebagai **Asisten Pembimbing Penulisan Ilmiah Tugas Akhir (Skripsi)** yang menjawab pertanyaan mahasiswa berdasarkan dokumen pedoman resmi.

---

## 🏗️ Arsitektur & Alur Kerja Workflow

Workflow ini terdiri dari dua alur utama:

### 1. Ingestion Pipeline (Proses Embed Dokumen ke Supabase)
- **Manual Trigger** (`When clicking ‘Execute workflow’`): Memulai proses ingest dokumen secara on-demand.
- **Google Drive Node** (`Download file`): Mengunduh file dokumen acuan (misal: *Panduan Penulisan Ilmiah Tugas Akhir Skripsi.pdf*).
- **Default Data Loader**: Mengekstrak teks dari data binary dokumen PDF.
- **Google Gemini Embeddings**: Mengonversi teks menjadi representasi vektor numerik.
- **Supabase Vector Store (Insert Mode)**: Menyimpan embeddings dan teks ke dalam tabel vektor Supabase (`documents`).

### 2. RAG Chatbot Pipeline (AI Agent Penjawab Pertanyaan)
- **Chat Trigger** (`When chat message received`): Menyediakan UI Web Chat publik untuk pengguna.
- **AI Agent**: Mengarahkan proses penalaran agent dengan persona pembantu penulisan ilmiah TA.
- **Google Gemini Chat Model**: Model LLM pemroses inferensi percakapan.
- **Supabase Vector Store Tool (Retrieve-as-Tool)**: Tool khusus agent untuk mencari dan mengambil potongan referensi panduan dari Supabase.
- **Postgres Chat Memory**: Menyimpan memori histori percakapan langsung ke PostgreSQL Supabase.

---

## 📁 Berkas Proyek

- [`Bot AI Menggunakan data supabase.json`](./Bot%20AI%20Menggunakan%20data%20supabase.json): File workflow n8n lengkap siap diimpor.

---

## 🚀 Panduan Import ke n8n

1. Buka instance **n8n** Anda.
2. Buat workflow baru atau masuk ke menu **Workflows**.
3. Klik tombol menu **Options** (ikon titik tiga di kanan atas) lalu pilih **Import from File...** (atau copy isi berkas JSON dan paste langsung di canvas).
4. Pilih berkas [`Bot AI Menggunakan data supabase.json`](./Bot%20AI%20Menggunakan%20data%20supabase.json).
5. Konfigurasikan kredensial yang dibutuhkan:
   - **Google Gemini (PaLM) API**: Untuk node Chat Model dan Embeddings.
   - **Supabase API**: Untuk koneksi ke database vektor Supabase.
   - **Postgres Database Credentials**: Untuk node Postgres Chat Memory.
   - **Google Drive OAuth2 API**: Untuk mengunduh dokumen panduan PDF.
6. Jalankan pipeline ingest untuk memasukkan dokumen ke Supabase, lalu aktifkan workflow untuk menggunakan Web Chatbot publik.

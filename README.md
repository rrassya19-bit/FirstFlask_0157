# First Project Flask

Aplikasi web sederhana untuk menerima nama dan menampilkan sapaan menggunakan Flask.

## Deskripsi

Project ini adalah aplikasi web sederhana yang dibuat dengan Flask. Aplikasi menerima input nama lewat form dengan method POST, lalu menampilkan sapaan di halaman yang sama. Jika nama dikirim dalam keadaan kosong, aplikasi menampilkan pesan peringatan. Project ini dibuat sebagai tugas "First Project Flask".

## Tujuan Pembuatan

- Memahami dasar penggunaan Flask untuk membuat aplikasi web.
- Memahami route dengan method GET dan POST.
- Memahami cara mengambil data dari form dengan `request.form`.
- Memahami cara menampilkan data memakai template Jinja2.

## Fitur

- Form input nama (TextField).
- Tombol Kirim (Button).
- Pesan hasil (Label).
- Pesan peringatan jika nama kosong.
- Tampilan yang diatur dengan CSS.

## Teknologi yang Digunakan

- Python 3.14.10
- Flask 3.1.3
- HTML dengan template Jinja2
- CSS
- Visual Studio Code

## Struktur Folder

```text
FirstFlask_0157/
├── app.py
├── requirements.txt
├── static/
│   └── style.css
└── templates/
    └── index.html
```

- `app.py`: file utama yang berisi kode aplikasi Flask, route, dan logika pemrosesan form.
- `requirements.txt`: daftar dependensi Python yang dibutuhkan project.
- `static/`: folder untuk file statis seperti CSS.
- `static/style.css`: file CSS yang mengatur tampilan halaman.
- `templates/`: folder untuk file template HTML.
- `templates/index.html`: template halaman utama yang menampilkan form dan pesan.

## Cara Kerja Aplikasi

1. Browser membuka alamat "/" dengan method GET, lalu Flask menampilkan form kosong.
2. Pengguna mengisi nama pada form dan menekan tombol Kirim, sehingga browser mengirim request POST ke "/".
3. Fungsi `index()` di `app.py` membaca nama dengan `request.form["nama"]`.
4. Jika nama kosong, pesan yang dibuat adalah peringatan. Jika ada isinya, pesan yang dibuat adalah sapaan.
5. Pesan dikirim ke `index.html` lewat `render_template` dan ditampilkan dengan `{{ pesan }}`.

## Penjelasan Kode

### app.py

Bagian import dan pembuatan aplikasi:

```python
from flask import Flask, render_template, request

app = Flask(__name__)
```

Baris pertama mengambil `Flask` untuk membuat aplikasi, `render_template` untuk menampilkan file template, dan `request` untuk membaca data yang dikirim browser. Baris `app = Flask(__name__)` membuat objek aplikasi Flask.

Bagian route dan pengecekan method:

```python
@app.route("/", methods=["GET", "POST"])
def index():
    pesan = ""
    if request.method == "POST":
        nama = request.form["nama"]
        if nama == "":
            pesan = "Silakan masukkan nama dulu"
        else:
            pesan = f"Halo, {nama}!"
    return render_template("index.html", pesan=pesan)
```

`@app.route("/", methods=["GET", "POST"])` mendaftarkan alamat "/" agar bisa diakses dengan GET maupun POST. Variabel `pesan` disiapkan dengan nilai kosong. Saat method-nya POST, nama diambil dari form memakai `request.form["nama"]`. Kalau nama kosong, `pesan` diisi peringatan; kalau ada isinya, `pesan` diisi sapaan. Terakhir, `render_template` mengirim `pesan` ke `index.html`.

Bagian penjalanan aplikasi:

```python
if __name__ == "__main__":
    app.run(debug=True)
```

Blok ini membuat aplikasi berjalan langsung saat file `app.py` dieksekusi, dengan mode debug aktif.

### templates/index.html

Bagian form:

```html
<form method="post">
    <label>Masukkan nama:</label>
    <input type="text" name="nama">
    <button>Kirim</button>
</form>
```

Form memakai `method="post"` sehingga data dikirim lewat POST. Input teks diberi `name="nama"` agar nilainya bisa dibaca di server, dan tombol Kirim dipakai untuk mengirim form.

Bagian pesan:

```html
{% if pesan != "" %}
    <p class="hasil">{{ pesan }}</p>
{% endif %}
```

Blok `{% if %}` hanya menampilkan paragraf pesan kalau `pesan` tidak kosong. Isi pesan ditampilkan dengan `{{ pesan }}`.

**Penting:** atribut `name="nama"` di HTML harus sama dengan kunci di `request.form["nama"]` pada `app.py`. Kalau namanya berbeda, data nama tidak akan terbaca oleh server.

### static/style.css

File CSS ini mengatur tampilan halaman: jenis huruf dan warna latar halaman, kotak putih di tengah dengan lebar dan jarak tertentu, ukuran input, warna dan bentuk tombol, serta warna hijau dan huruf tebal untuk teks hasil.

## Prasyarat

- Python 3 sudah terpasang.
- pip tersedia.
- Browser untuk membuka halaman web.

## Cara Instalasi dan Menjalankan

1. Clone repository:

   ```bash
   git clone https://github.com/[username]/FirstFlask_0157.git
   ```

2. Masuk ke folder project:

   ```bash
   cd FirstFlask_0157
   ```

3. Buat virtual environment:

   ```bash
   python3 -m venv venv
   ```

4. Aktifkan virtual environment.

   macOS/Linux:

   ```bash
   source venv/bin/activate
   ```

   Windows:

   ```bash
   venv\Scripts\activate
   ```

5. Install dependensi:

   ```bash
   pip install -r requirements.txt
   ```

6. Jalankan aplikasi:

   ```bash
   flask --app app run --debug
   ```

7. Buka `http://127.0.0.1:5000` di browser.

Untuk menghentikan server, tekan `Ctrl+C` di terminal. Untuk keluar dari virtual environment, jalankan perintah `deactivate`.

## Cara Penggunaan

1. Buka halaman `http://127.0.0.1:5000` di browser.
2. Isi nama pada kolom input.
3. Klik tombol Kirim.
4. Lihat pesan sapaan yang muncul di bawah form.
5. Coba juga mengosongkan kolom nama lalu klik Kirim untuk melihat pesan peringatan.

## Contoh Hasil

- Kondisi awal (GET): form tampil tanpa pesan.
- Input "Budi": tampil "Halo, Budi!".
- Input kosong: tampil "Silakan masukkan nama dulu".

[Tempel screenshot tampilan awal di sini]
[Tempel screenshot hasil input nama di sini]
[Tempel screenshot input kosong di sini]

## Troubleshooting

- **Error 405 Method Not Allowed**: route belum punya `methods=["GET", "POST"]`. Tambahkan pada dekorator `@app.route`.
- **Error TemplateNotFound**: folder harus bernama `templates` (pakai s) dan sejajar dengan `app.py`.
- **Data nama tidak terbaca atau muncul error 400**: atribut `name` di input tidak sama dengan kunci di `request.form`. Pastikan keduanya memakai `nama`.
- **CSS tidak berubah**: refresh paksa browser dengan `Cmd+Shift+R` di macOS atau `Ctrl+Shift+R` di Windows.
- **Perintah flask tidak dikenali**: virtual environment belum aktif atau Flask belum diinstall. Aktifkan venv lalu jalankan `pip install -r requirements.txt`.
- **Port 5000 sedang dipakai**: hentikan server lain atau jalankan dengan `flask --app app run --debug --port 5001`.

## Catatan Pembelajaran

Dari tugas ini saya belajar bagaimana alur GET dan POST bekerja pada aplikasi Flask: GET dipakai saat halaman pertama kali dibuka, lalu POST dipakai saat form dikirim. Saya juga belajar bahwa atribut `name` pada input form harus sesuai dengan kunci yang dibaca di `request.form`, karena kalau tidak sesuai datanya tidak terbaca. Selain itu, pemakaian template membuat kode HTML tetap terpisah dari kode Python sehingga lebih rapi.

## Referensi

- Flask Quickstart: https://flask.palletsprojects.com
- Materi kuliah "Introduction to Flask" (Lesson 1)

## Identitas Pembuat

- Nama: [isi nama]
- NIM: [isi NIM]
- Kelas: [isi kelas]
- Mata kuliah: [isi mata kuliah]
- Dosen pengampu: [isi nama dosen]
- Universitas: Universitas Muhammadiyah Yogyakarta

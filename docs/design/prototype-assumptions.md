# Asumsi Prototipe v3.3

Dokumen status v3.3 cukup untuk dibaca manusia, tapi belum cukup untuk ditulis jadi kode. Setiap baris di bawah adalah pertanyaan yang harus punya jawaban sebelum core sim bisa jalan. Default dipilih yang paling sederhana dan paling dekat dengan bunyi dokumen status. Semua default berupa parameter di data, jadi bisa diubah tanpa menyentuh kode.

Status: menunggu Upi. Upi cukup menulis "setuju" atau koreksi per baris. Kalau ada ambiguitas baru saat coding, tambahkan baris baru dengan default dan status "menunggu Upi", jangan menebak diam-diam.

## Scope grey-box

A0. Grey-box pertama memakai 4 spirit per tim (2 di arena, 2 di bench), tanpa draft dan ban, tanpa skill Area, tanpa Resonansi. Alasannya: rotasi bench adalah salah satu hal yang mau diuji, sedangkan Area dan Resonansi menambah variabel sebelum inti terbukti. Draft, ban, Area, dan Resonansi masuk setelah playtest pertama.

## Pertanyaan yang harus dijawab

| ID | Pertanyaan | Kenapa penting | Default usulan |
| --- | --- | --- | --- |
| A1 | Apakah Ready boleh ditahan? Berapa lama? | v1 membuktikan menahan Ready tanpa batas jadi strategi dominan. Di sisi lain, tanpa menahan Ready, Lelah tidak bisa dihukum di 1v1 (review 2.2) | Boleh ditahan maksimal 3 detik, lalu otomatis Charge. Parameter: `readyHoldTicks` (0 = tidak bisa ditahan) |
| A2 | Beban reaction 20 detik itu jendela bergulir, atau reset setelah 20 detik tanpa reaksi? | Kalau bergulir, spirit lambat bisa tidak pernah kena Lelah di 1v1 | Beban menumpuk, dan reset ke 0 setelah 20 detik tanpa bereaksi. Paling sesuai dengan bunyi "Lelah hilang setelah 20 detik tanpa bereaksi" |
| A3 | Mundur 50% dan 75% itu dari panjang track penuh, atau dari posisi saat ini? | Bereaksi di posisi 60% bisa berakhir di 10% atau di 30% | Dikurangi dari panjang track penuh, minimal 0. Bereaksi di 60% berarti mundur ke 10% |
| A4 | Apa efek satu layer terhadap layer di bawahnya? Tipe apa yang sah untuk layer kedua dan seterusnya? | Menentukan apakah chain diselesaikan paritas (review 2.1). Dokumen status hanya mendefinisikan reaksi terhadap Skill Utama | Setiap layer meniadakan layer tepat di bawahnya. Layer kedua dan seterusnya boleh memakai reaction apa pun yang dibawa spirit itu, tanpa cek tipe. Resolver dibuat bisa diganti |
| A5 | Apakah Lelah mereset hitungan beban saat habis? | Menentukan berapa reaksi gratis setelah Lelah | Ya, beban kembali ke 0 saat Lelah selesai |
| A6 | Apa yang tetap berjalan selama slow-mo chain? | "Arena tetap hidup" belum menyebut icon, Energy, dan auto-attack | Semua berjalan di 10% (icon, gerak, auto-attack, Energy, durasi status). Regen mana 0 |
| A7 | Bagaimana Speed diubah menjadi panjang siklus? | Dokumen hanya menyebut 8 sampai 11 detik | Linear dari Speed minimum ke maksimum, dibulatkan ke tick: 80 sampai 110 tick |
| A8 | Berapa HP, damage Skill Utama, dan damage auto-attack? | Stat disamakan, tapi angka dasarnya tidak ada | Skill Utama = 100 satuan, HP = 550 (sekitar 4 Skill Utama yang mendarat, sisanya dari damage otomatis). Auto-attack disetel sampai damage otomatis 20 sampai 25%. Disetel ulang lewat sim agar match 5 sampai 7 menit |
| A9 | Seperti apa penskalaan damage di dalam chain "model FaB dan BlazBlue"? | Tidak ada angka | Mati untuk prototipe (100%). Parameter tetap ada |
| A10 | Bagaimana modifier damage menumpuk (Vulnerable +30%, Lelah +20%, Fase Akhir +15% per 30 detik)? | Additive dan multiplicative memberi hasil berbeda | Modifier damage diterima dijumlahkan (Vulnerable + Lelah = +50%). Fase Akhir dikalikan terpisah sebagai pengali global |
| A11 | Kapan tepatnya Last Stand aktif? | Dokumen menyebut "lawan masih punya 2 spirit" sekaligus "berlaku di 1v1" | Aktif saat tim hanya punya 1 spirit tersisa (arena dan bench digabung), berapa pun sisa lawan |
| A12 | Siapa memilih target Skill Utama? | Mempengaruhi siapa yang bisa menjawab | Pemain memilih saat aktivasi, dengan saran default target AI saat itu |
| A13 | Siapa memilih pengganti setelah KO? | Dokumen hanya menyebut waktu masuk | Pemain memilih dalam 3 detik. Kalau tidak memilih, spirit pertama di urutan bench masuk |
| A14 | Berapa laju Energy per role? | Dokumen hanya menyebut sumbernya | Skill Auto keluar rata-rata setiap 12 detik untuk semua role. Laju per sumber disetel lewat sim agar rata-ratanya sama |
| A15 | Apakah bot dan pemain boleh pass di giliran pertama pihak bertahan? | Menentukan apakah membiarkan serangan masuk itu pilihan | Ya. Pass di giliran pertama berarti serangan langsung masuk tanpa slow-mo berlanjut |

## Catatan implementasi

Semua angka di atas masuk ke satu file konfigurasi aturan. Hipotesis dari [backlog](hypothesis-backlog.md) diimplementasikan sebagai toggle yang mati secara default. Default hanya boleh diubah oleh Upi, satu per putaran playtest.

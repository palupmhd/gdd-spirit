# Chain dan Tipe

Status: arah disetujui Upi (keputusan D1). Angka masih usulan dan disetel lewat sim. Bagian "tipe sebagai efisiensi" masih menunggu persetujuan Upi. Dokumen ini menggantikan bagian Chain, Skill Utama dan tipe serangan, dan Burst di v3.3, serta asumsi A4 dan A17.

## Intinya

Saat lawan mengeluarkan Skill Utama, serangan itu membawa Power (kekuatan serangan). Kedua pemain bergantian menaruh kartu. Pihak bertahan menaruh tangkisan yang menambah Tahan (pengurang damage). Pihak penyerang menaruh Tekan yang menambah Power. Setiap kartu memakai mana. Begitu ada yang pass, damage dihitung: Power dikurangi Tahan.

Tidak ada kartu yang membatalkan kartu lain. Pertanyaan pemain bukan lagi "siapa punya kartu terakhir", tapi "berapa mana yang layak aku bakar untuk serangan ini, padahal aku butuh mana untuk serangan berikutnya". Ini cara kerja Flesh and Blood: setiap kartu yang dipakai bertahan adalah kartu yang tidak bisa dipakai menyerang.

## Tiga tipe serangan

Tipe tetap ditentukan oleh cara serangan mencapai target. Bedanya dengan v3.3: tipe sekarang menentukan siapa yang boleh menangkis dan tangkisan mana yang paling efisien, bukan boleh atau tidaknya menangkis.

| Tipe | Cara mencapai target | Yang boleh menangkis | Tangkisan cocok | Ciri khas |
| --- | --- | --- | --- | --- |
| Kontak | Tubuh penyerang bergerak ke target | Hanya target | Dodge | Partner tidak bisa membantu. Senjata untuk menghukum spirit yang mengandalkan perlindungan partner |
| Lontaran | Proyektil atau gelombang energi | Target atau partner | Shield | Mudah dilindungi tim yang punya Shield di depan |
| Rapalan | Efek muncul di target setelah dirapal | Target atau partner | Interrupt | Paling mahal dijawab, karena Interrupt berbiaya 3 mana |

## Tipe sebagai efisiensi (solusi lubang tipe)

Masalahnya: di v3.3, spirit yang tidak membawa tangkisan yang cocok tidak bisa menjawab sama sekali. Sim M1 mencatat sekitar 30% serangan masuk karena lubang ini, dan untuk Kontak, partner pun tidak bisa membantu.

Riset dari game lain:

| Game | Cara menangani serangan yang "salah lawan" | Pelajaran |
| --- | --- | --- |
| Flesh and Blood | Hampir semua kartu punya nilai defense, jadi selalu bisa blok dengan kartu apa pun. Defense reaction khusus lebih efisien | Bertahan selalu mungkin, spesialisasi memberi efisiensi |
| Pokémon | Type chart adalah pengali damage (2x, 0,5x), bukan izin. Protect menahan hampir semua serangan, tapi peluangnya turun drastis kalau dipakai berturut-turut | Tipe sebagai efisiensi sudah dipahami jutaan pemain creature game |
| Fighting game (SF6) | Block selalu tersedia tapi tetap kena chip damage. Drive Parry memberi hadiah lebih besar | Pilihan aman dan pilihan serakah |
| Octopath Traveler, Persona | Kelemahan memberi bonus besar, bukan syarat untuk bisa menyerang | Kecocokan sebagai bonus terasa memuaskan tanpa menghukum yang tidak cocok |
| League of Legends | Flash: tombol kabur universal dengan cooldown panjang, dibawa hampir semua champion | Jawaban universal yang mahal mencegah rasa tidak berdaya |

Solusi terbaik untuk game ini: tangkisan yang tidak cocok tipe tetap boleh dipakai, dengan nilai separuh. Karena setiap spirit membawa 2 tangkisan, target selalu punya jawaban selama icon, mana, dan statusnya memungkinkan. Tidak perlu mekanik baru seperti Tahan (H13), karena model pengurangan damage sudah menyediakan "jawaban separuh" secara alami. Celah yang tersisa hanya celah hasil permainan: icon belum lewat garis tengah, mana habis, stun, atau Lelah.

## Kartu di chain

| Kartu | Dipakai oleh | Biaya | Nilai | Catatan |
| --- | --- | --- | --- | --- |
| Dodge | Pihak bertahan | 2 mana | Tahan 60 vs Kontak, 30 vs tipe lain | Slot tangkisan |
| Shield | Pihak bertahan | 2 mana | Tahan 60 vs Lontaran, 30 vs tipe lain | Slot tangkisan |
| Interrupt | Pihak bertahan | 3 mana | Tahan 60 vs Rapalan, 30 vs tipe lain | Slot tangkisan |
| Tekan | Pihak penyerang: si penyerang dan partnernya | 2 mana | Power +30 | Dimiliki semua spirit, tidak butuh slot |
| Burst | Controller, di pihak mana pun | 4 mana | Tahan 60 (kalau bertahan) atau Power +60 (kalau menyerang), lalu chain langsung selesai | Mengisi 1 slot tangkisan, 2 beban |

Power dasar Skill Utama adalah 100 (asumsi A8). Efisiensi per mana: tangkisan cocok 30 per mana (Interrupt 20), Tekan dan tangkisan tidak cocok 15 per mana. Artinya membawa tangkisan yang tepat tetap bernilai, tapi Tekan cukup untuk menembus tangkisan yang salah.

## Aturan chain

1. Chain terbuka kalau pihak bertahan punya minimal satu kartu yang boleh dipakai: spirit yang boleh menangkis sesuai tipe, icon sudah lewat garis tengah, tidak stun atau Lelah, dan mana cukup.
2. Waktu melambat ke 10% dan regen mana berhenti, sama seperti v3.3.
3. Pihak bertahan bergerak pertama, lalu bergantian. Setiap giliran: taruh 1 kartu atau pass.
4. Satu spirit boleh menaruh lebih dari satu kartu dalam satu chain. Syarat icon dicek sekali saat chain terbuka.
5. Setiap kartu menambah 1 beban (Burst 2). Kartu yang membuat beban mencapai 3 boleh dipakai, tapi spirit itu tidak boleh menaruh kartu lagi di chain ini, dan Lelah mulai berlaku setelah chain selesai.
6. Si penyerang boleh menaruh Tekan untuk serangannya sendiri walaupun icon-nya sudah kembali ke awal. Partner penyerang butuh icon lewat garis tengah seperti biasa.
7. Mundurnya icon dijumlahkan dan diterapkan setelah chain selesai: 50% untuk kartu pertama dalam 20 detik, 75% untuk kartu kedua, seperti v3.3. Icon si penyerang tidak mundur karena sudah di awal.
8. Chain selesai saat pihak yang dapat giliran pass, atau saat Burst dipasang.
9. Damage = Power total dikurangi Tahan total, minimal 10 (chip damage). Hasilnya lalu dikali modifier: Vulnerable, Lelah, dan Fase Akhir.
10. Tertahan penuh: kalau Tahan sama atau lebih besar dari Power, dan minimal satu kartu yang cocok tipe ikut dipakai, penyerang jadi Vulnerable 2 detik. Efek tambahan Skill Utama (misalnya stun) hanya terjadi kalau serangan tidak tertahan penuh.
11. Bank waktu 35 detik per match tetap berlaku.

## Contoh

Tim kamu: Karang (Vanguard, membawa Shield dan Interrupt) dan Bara (Diver, membawa Dodge dan Shield). Tim lawan: Petir (Striker) dan Kabut (Controller, membawa Interrupt dan Burst). Mana kamu 7, mana lawan 8.

Petir memakai Skill Utama Lontaran ke Bara dengan Power 100. Mana lawan turun ke 5.

| Giliran | Aksi | Power | Tahan | Damage kalau selesai sekarang | Mana kamu | Mana lawan |
| --- | --- | --- | --- | --- | --- | --- |
| Kamu | Karang menaruh Shield (cocok, Tahan +60) | 100 | 60 | 40 | 5 | 5 |
| Lawan | Petir menaruh Tekan (Power +30) | 130 | 60 | 70 | 5 | 3 |
| Kamu | Bara menaruh Shield (cocok, Tahan +60) | 130 | 120 | 10 | 3 | 3 |
| Lawan | Kabut menaruh Tekan (Power +30) | 160 | 120 | 40 | 3 | 1 |
| Kamu | Pass, supaya 3 mana tersisa untuk serangan berikutnya | | | | | |

Hasilnya Bara menerima 40 damage, bukan 100. Kamu membakar 4 mana untuk mencegah 60 damage. Lawan membakar total 7 mana (termasuk Skill Utama) untuk mendaratkan 40. Tidak ada yang dibatalkan, setiap kartu mengubah angka, dan setiap giliran adalah keputusan ekonomi. Ini mirip menghitung "elixir trade" di Clash Royale.

Variasi: kalau lawan masih punya 4 mana di giliran keempat, Kabut bisa memasang Burst (Power +60). Damage jadi 70, dan chain langsung selesai tanpa kamu bisa membalas.

## Kenapa ini menyelesaikan masalah lama

| Masalah lama | Kenapa hilang atau mengecil |
| --- | --- |
| Pihak bertahan selalu menang chain penuh | Tidak ada pemenang biner. Hasilnya angka yang bergantung pada mana yang dibakar kedua pihak |
| Hasil chain bisa dihitung sebelum mulai | Nilai sebuah hasil bergantung pada rencana mana tiap pemain, jadi tidak bisa dibaca dari layar saja. Tetap diukur lewat metrik prediktabilitas chain |
| Lubang tipe sekitar 30% | Tangkisan tidak cocok tetap boleh dipakai dengan nilai separuh |
| Penyerang tidak punya cara mendorong | Tekan memberi penyerang dan partnernya cara menambah Power |
| Dua pemain sabar macet | Serangan hampir selalu menghasilkan damage, minimal chip, jadi menunggu celah sempurna kurang menarik |

## Risiko yang harus diukur

1. Slow-mo terlalu sering. Karena tangkisan tidak cocok tetap sah, chain akan lebih sering terbuka dibanding v3.3. Perkiraan kasar: satu Skill Utama setiap 4 detik dari kedua pihak. Mitigasi berurutan, dari yang paling ringan:
   - Toggle auto-pass per spirit dan aturan sederhana, misalnya "jangan tawarkan chain kalau mana di bawah 3".
   - Batas waktu keputusan pertama pihak bertahan 2 detik waktu nyata, lalu otomatis pass.
   - Kalau masih terlalu sering: pihak bertahan harus menekan tombol tangkis selama ancang-ancang (tanpa slow-mo), dan slow-mo baru dimulai setelah itu. Ini menambah unsur refleks, jadi hanya dipakai kalau dua mitigasi pertama gagal.
   - Target: maksimal 6 chain per menit.
2. Chain terlalu panjang. Mana maksimal 10 dan beban membatasi sekitar 4 kartu per pihak. Ukur rata-rata durasi slow-mo per chain, targetnya di bawah 6 detik waktu nyata.
3. Angka. 60, 30, Tekan 30, dan chip 10 hanya titik awal. Yang dijaga: tangkisan cocok harus lebih efisien per mana dari Tekan, dan Tekan harus cukup untuk menembus tangkisan yang tidak cocok.

## Tampilan di layar

Panel chain menampilkan bar tarik-menarik: Power di satu sisi, Tahan di sisi lain, dan angka besar "damage kalau selesai sekarang" di tengah. Setiap kartu yang ditaruh menggeser bar dengan animasi. Sisa mana kedua pihak tampil di samping panel. Pemain tidak perlu menghitung di kepala, jadi keputusannya murni soal nilai, bukan aritmetika.

## Istilah kode

| Desain | Kode |
| --- | --- |
| Power | `Power` |
| Tahan | `Block` |
| Tekan | `Press` |
| Chip damage | `ChipDamage` |
| Tertahan penuh | `FullBlock` |
| Tangkisan cocok tipe | `TypeMatch` |

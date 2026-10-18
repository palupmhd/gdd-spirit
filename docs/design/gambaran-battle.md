# Gambaran Battle

Dokumen ini menggambarkan seperti apa satu battle terlihat dan terasa, dengan bahasa sederhana. Aturannya mengikuti [v3.3](v3.3-status.md) dan default di [asumsi prototipe](prototype-assumptions.md). Tata letak layar, sudut kamera, dan warna adalah usulan visual, bukan aturan, jadi bebas diubah.

Catatan: sebagian isi dokumen ini sudah berubah lewat [log keputusan](keputusan.md). Chain sekarang mengurangi damage, bukan membatalkan (lihat [chain dan tipe](chain-dan-tipe.md)), jadi contoh chain di bawah memakai model lama. Match selesai di 3 KO, Last Stand dihapus, dan spirit di bench memulihkan luka ringan. Gaya visual masih dikaji di [arah visual](arah-visual.md).

## Sekilas

Bayangkan kamu menonton dua pasang spirit bertarung sendiri di sebuah arena kecil. Mereka bergerak, memukul, dan mengeluarkan jurus kecil tanpa kamu sentuh. Tugasmu seperti pelatih di pinggir lapangan yang memegang tiga tombol penting: kapan jurus besar keluar, kapan menangkis serangan lawan, dan kapan mengganti pemain.

Setiap kali lawan mengeluarkan jurus besar dan kamu bisa menangkisnya, waktu melambat hampir berhenti. Kamu dan lawan bergantian menaruh kartu jawaban, seperti adu kartu singkat di tengah pertarungan. Setelah itu waktu kembali normal dan pertarungan lanjut.

Satu match berlangsung 5 sampai 7 menit dan selesai saat salah satu tim kehilangan keempat spiritnya.

## Sebelum battle: memilih tim

1. Kedua pemain melihat 6 spirit milik masing-masing (Team Preview, layar pratinjau tim).
2. Kamu mencoret 1 spirit milik lawan (ban, larangan main), lawan juga mencoret 1 milikmu.
3. Dari 5 yang tersisa, kamu memilih 4 untuk dibawa. Dua di antaranya jadi lead (spirit pembuka yang langsung turun ke arena), dua lagi menunggu di bench (bangku cadangan).
4. Setiap spirit sudah membawa 1 jurus besar dan 2 jenis tangkisan yang dipilih sebelum antre (loadout, perlengkapan yang dikunci). Di ranked (mode peringkat), lawan tidak tahu tangkisan apa yang kamu bawa.

## Yang terlihat di layar

```
+------------------------------------------------------------------+
| 4:12                                          MANA LAWAN ●●●●●○○○○○ |
|                                                                  |
|  TIMELINE  |----[a]-------[B]--|--[A]--------[b]----| READY      |
|                                ^ garis tengah                    |
|                                                                  |
|  BENCH                                               BENCH LAWAN |
|  [C] [D]                                                [c] [d]  |
|                                                                  |
|              B (belakang)                  b (belakang)          |
|                      A (depan)   >  <  a (depan)                 |
|                                                                  |
|                                                                  |
| MANA KAMU ●●●●●●●○○○                                              |
|                     [ JURUS BESAR ] [ CHARGE ] [ PERFECT SWAP ]  |
|                     (tombol hanya muncul saat spirit Ready)      |
+------------------------------------------------------------------+
```

Huruf besar (A, B, C, D) adalah spirit kamu, huruf kecil milik lawan.

- Arena (usulan): 2D dengan kamera samping. Tim kamu di kiri menghadap kanan, lawan di kanan menghadap kiri. Setiap tim punya posisi depan (dekat tengah) dan belakang (sedikit lebih tinggi dan lebih jauh), dan spirit hanya bergerak ke kiri dan kanan. Detailnya ada di [diskusi 2026-10-18](diskusi-2026-10-18.md), poin 4.
- Di atas kepala setiap spirit ada bar HP (nyawa), bar Energy kecil (pengisi jurus otomatis), titik beban (berapa kali spirit itu baru saja menangkis), dan ikon status seperti stun (pingsan sesaat).
- Timeline (jalur giliran) ada di atas. Keempat ikon spirit di arena berjalan dari kiri ke kanan di satu jalur yang sama.
- Mana kamu ada di bawah, mana lawan di kanan atas. Kedua pemain bisa melihat mana satu sama lain.
- Bench ada di pinggir. Spirit di bench terlihat, lengkap dengan HP-nya.

## Yang terjadi sendiri, dan yang kamu atur

Spirit bergerak dan menyerang sendiri sesuai role (peran) masing-masing. Dari kejauhan, setiap role terlihat beda:

| Role | Yang terlihat di arena |
| --- | --- |
| Vanguard (penjaga depan) | Maju paling depan, menahan lawan, menghalangi jalan ke partnernya |
| Striker (penembak) | Berdiri di belakang dan menyerang terus dari jarak jauh |
| Diver (penyusup) | Melompat melewati spirit depan lawan untuk mengejar lawan yang berdiri di belakang |
| Controller (pengatur medan) | Memasang zona, misalnya area yang membuat lawan lambat, dan mengganggu lawan |
| Support (pendukung) | Menjaga dan memulihkan partner, menjauh dari bahaya |

Yang berjalan otomatis:

- Auto-attack (pukulan biasa yang keluar sendiri).
- Skill Auto (jurus kecil otomatis). Keluar sendiri saat bar Energy penuh. Efeknya kecil dan tidak bisa membuat lawan KO (kalah dan keluar arena). Energy Vanguard terisi saat dia dipukul, Striker dan Diver saat mereka memukul, Controller dan Support terisi seiring waktu.

Yang kamu atur:

- Jurus besar, Charge, atau Perfect Swap, saat spirit Ready.
- Tangkisan, saat lawan mengeluarkan jurus besar.
- Swap Darurat (ganti pemain cepat tanpa efek), kapan saja di luar adu tangkisan.
- Resonansi (mode kuat sekali per match).

## Timeline: kapan boleh bergerak

Bayangkan lomba lari mini di bagian atas layar. Keempat ikon spirit berlari dari kiri ke kanan. Spirit yang Speed-nya (kecepatan) tinggi menyelesaikan satu putaran sekitar 8 detik, yang lambat sekitar 11 detik.

- Saat ikon melewati garis tengah, ikon itu menyala. Artinya spirit itu sudah bisa menangkis.
- Saat ikon sampai di ujung kanan, spirit itu Ready (siap beraksi). Tombol aksi muncul, dan kamu memilih satu: jurus besar, Charge, atau Perfect Swap. Setelah itu ikon kembali ke kiri.
- Setiap kali spirit menangkis, ikonnya terlempar mundur. Tangkisan pertama memundurkan setengah jalur, tangkisan kedua tiga perempat jalur. Jadi menangkis selalu ada harganya: jurus besar spirit itu jadi tertunda.
- Tidak ada jurus yang bisa memundurkan atau memajukan ikon lawan. Kecepatan penting, tapi tidak jadi segalanya.

## Mana: dompet bersama

Mana terlihat seperti 10 butir kristal. Kedua spirit kamu memakai dompet yang sama.

- Match dimulai dengan 5 butir. Setiap 2 detik bertambah 1, maksimal 10. Kalau sudah penuh, mana yang masuk terbuang. Jadi menimbun terlalu lama itu rugi.
- Jurus besar dan tangkisan memakai mana. Biaya jurus besar belum ditentukan di v3.3, untuk sementara anggap 3 butir.
- Charge (mengisi mana): saat Ready, spirit tidak menyerang tapi menambah 2 atau 3 butir mana. Spirit yang lambat mendapat lebih banyak.
- Karena mana lawan terlihat, kamu bisa membaca situasi. Lawan tinggal 1 butir? Dia tidak bisa menangkis apa pun. Itu saat yang tepat untuk menyerang.

## Jurus besar dan tiga jenis serangan

Saat spirit mengeluarkan jurus besar (Skill Utama), ada ancang-ancang singkat (wind-up) dengan warna yang menunjukkan jenisnya. Warna di bawah adalah usulan. Sebaiknya setiap jenis juga punya bentuk ikon sendiri, supaya pemain buta warna tetap bisa membedakannya.

| Jenis serangan | Seperti apa | Warna (usulan) | Ditangkis dengan | Siapa yang boleh menangkis |
| --- | --- | --- | --- | --- |
| Kontak | Tubuh penyerang melesat ke target, misalnya terjangan atau cakaran | Oranye | Dodge (menghindar), 2 mana | Hanya target |
| Lontaran | Peluru, bola energi, atau gelombang yang terbang ke target | Biru | Shield (perisai), 2 mana | Target atau partnernya |
| Rapalan | Mantra yang dibaca dulu, lalu efeknya muncul di tubuh target | Ungu | Interrupt (memotong mantra), 3 mana | Target atau partnernya |

Ada satu jenis tangkisan khusus, Burst (ledakan penutup), yang hanya dimiliki Controller dan butuh 4 mana. Burst tidak bisa dijawab dan langsung mengakhiri adu tangkisan.

## Momen paling seru: chain (adu tangkisan)

Chain hanya terbuka kalau kamu memang bisa menangkis: ikon spiritmu sudah lewat garis tengah, mananya cukup, spiritnya tidak sedang pingsan, dan jenis tangkisannya cocok. Kalau tidak bisa, serangan langsung masuk tanpa jeda.

Saat chain terbuka:

1. Waktu melambat jadi 10% dari kecepatan normal (slow-mo). Arena tetap bergerak sangat pelan, dan mana berhenti terisi.
2. Muncul tumpukan di tengah layar, seperti kartu yang ditaruh satu di atas yang lain. Kartu paling bawah adalah jurus besar si penyerang.
3. Kamu (pihak yang diserang) dapat giliran pertama. Pilih: taruh tangkisan di atas tumpukan, atau pass (tidak menjawab).
4. Lalu giliran lawan, dan seterusnya bergantian. Setiap spirit hanya boleh menaruh 1 kartu per chain, dan spirit yang sedang menyerang tidak boleh ikut.
5. Begitu ada yang pass, tumpukan diselesaikan dari kartu paling atas ke bawah. Kartu yang ditaruh terakhir bekerja duluan.
6. Kalau jurus besar berhasil ditangkis, penyerangnya jadi Vulnerable (rentan) selama 2 detik: dia menerima damage 30% lebih besar.

Setiap pemain punya jatah waktu berpikir 35 detik untuk seluruh match. Kalau habis, setiap giliran hanya diberi setengah detik.

### Contoh satu chain

Tim kamu: Karang (Vanguard) menahan di depan, Bara (Diver, cepat) melompat ke belakang lawan. Tim lawan: Petir (Striker) dan Kabut (Controller). Mana kamu 7, mana lawan 8.

1. Ikon Petir sampai di ujung. Lawan memakai jurus besar Petir ke Bara. Ancang-ancangnya berwarna biru, berarti Lontaran. Mana lawan turun jadi 5.
2. Karang membawa Shield, ikonnya sudah lewat garis tengah, dan mana kamu cukup. Chain terbuka dan layar melambat.
3. Giliranmu. Karang memasang Shield di depan Bara (kartu 1). Mana kamu turun jadi 5, dan ikon Karang terlempar mundur.
4. Giliran lawan. Kabut memasang Interrupt untuk membatalkan Shield itu (kartu 2). Mana lawan turun jadi 2.
5. Giliranmu. Ikon Bara juga sudah lewat garis tengah. Bara memasang Dodge untuk membatalkan Interrupt Kabut (kartu 3). Ini mengikuti asumsi A4: kartu kedua dan seterusnya boleh memakai tangkisan apa saja, tanpa cek jenis. Mana kamu turun jadi 3, dan ikon Bara terlempar mundur.
6. Giliran lawan. Petir sedang menyerang dan Kabut sudah menaruh kartu, jadi lawan tidak punya siapa-siapa lagi. Lawan pass.
7. Tumpukan diselesaikan dari atas: Dodge Bara membatalkan Interrupt Kabut, jadi Shield Karang tetap berdiri, dan tembakan Petir hancur di perisai. Petir jadi Vulnerable selama 2 detik.

Waktu kembali normal. Kamu menang chain, tapi lihat harganya: mana tinggal 3, dan ikon kedua spiritmu baru saja mundur, jadi tidak ada yang bisa memakai jurus besar untuk menghukum Petir yang sedang rentan. Yang menikmati bonus 30% itu hanya pukulan otomatis. Inilah inti game ini: setiap jawaban punya harga, dan kamu yang memutuskan kapan harga itu layak dibayar.

## Beban dan Lelah: menangkis terus itu melelahkan

Setiap kali spirit menangkis, satu titik beban di atas kepalanya menyala.

- Titik beban hilang kalau spirit itu tidak menangkis selama 20 detik.
- Tangkisan ketiga sebelum beban hilang membuat spirit Lelah (kehabisan napas). Secara visual spirit terlihat terengah dengan aura abu-abu. Selama 4 detik dia tidak bisa menangkis dan menerima damage 20% lebih besar, dan ikonnya kembali ke titik awal.
- Cara paling aman menghapus beban: tarik spirit itu ke bench. Setelah 5 detik di bench, bebannya bersih.

Ini membuat pemain berpikir: spirit mana yang sudah capek, dan apakah serangan kali ini layak ditangkis atau dibiarkan masuk saja?

## Ganti pemain

Ada dua cara mengganti spirit di arena dengan spirit di bench.

| Cara | Kapan | Biaya | Yang terjadi |
| --- | --- | --- | --- |
| Perfect Swap (ganti sempurna) | Saat spirit Ready | 2 mana | Spirit yang keluar memberi outro (efek perpisahan, misalnya buff atau penguat untuk tim). Spirit yang masuk melakukan intro (aksi saat mendarat, misalnya serangan kecil) dan ikonnya langsung mulai di 40% jalur |
| Swap Darurat (ganti cepat) | Kapan saja di luar chain | Gratis | Tanpa intro dan outro. Spirit yang masuk mulai dari ikon 0, dan baru bisa swap lagi setelah 5 detik |

Perfect Swap adalah momen paling sinematik di game ini, mirip pergantian karakter di Wuthering Waves: satu spirit melompat keluar sambil meninggalkan efek, yang lain mendarat sambil menyerang. Damage intro dibatasi (maksimal sepertiga jurus besar dan tidak bisa membuat KO), jadi tampil megah tanpa jadi cara curang untuk menang.

Perlu diingat, spirit di bench tidak memulihkan HP. Bench hanya untuk menghapus beban dan menyimpan spirit, bukan tempat istirahat untuk sembuh.

## KO dan pengganti

Saat HP spirit habis, dia KO dan keluar dari arena. Tiga detik kemudian kamu memilih pengganti dari bench. Pengganti masuk tanpa intro, tapi ikonnya langsung di 70% jalur, jadi dia cepat bisa beraksi.

## Resonansi: kartu as sekali per match

Sekali per match, satu spirit yang sedang Ready boleh beresonansi. Gratis, dan tidak menghabiskan giliran Ready-nya. Spirit itu diselimuti aura yang terlihat jelas. Sampai dia KO, jenis jurus besarnya berubah ke jenis yang dipilih saat menyusun tim, dan dia bisa memakai ketiga jenis tangkisan. Di ranked, lawan tidak tahu jenis apa yang akan muncul sampai Resonansi aktif.

## Fase Akhir dan Last Stand

Mulai menit 4:00, arena berubah suasana (usulan: cahaya memerah dan musik makin tegang). Mana terisi dua kali lebih cepat. Setiap 30 detik, semua damage naik 15%, sementara heal (pemulihan HP) dan perisai HP turun 15%. Ini mendorong match supaya cepat selesai. Tidak ada kemenangan karena waktu habis.

Saat sebuah tim tinggal punya 1 spirit, spirit itu masuk Last Stand (bertahan terakhir). Dia bisa memakai ketiga jenis tangkisan yang ada di pool (kumpulan tangkisan) miliknya, bukan hanya 2 yang dia bawa.

## Akhir match

Tim yang kehilangan keempat spiritnya kalah. Pemain yang tertinggal 2 KO atau lebih boleh menyerah (surrender).

## Rasanya satu menit bermain

Dalam satu menit, salah satu dari dua spiritmu Ready kira-kira setiap 4 sampai 5 detik, dan lawan juga sesering itu. Jadi kira-kira setiap 2 sampai 3 detik ada keputusan kecil: serang, Charge, ganti, tangkis, atau biarkan. Di antaranya kamu menonton spirit bertarung sendiri, sambil memantau ikon di timeline dan mana lawan.

Empat pertanyaan yang terus muncul di kepala pemain:

1. Spiritku Ready: serang sekarang, isi mana, atau ganti pemain?
2. Aku diserang: tangkis, atau biarkan masuk supaya mana dan tenaga tersimpan?
3. Di tengah chain: tambah kartu, atau pass? Siapa yang masih punya mana dan tangkisan?
4. Sepanjang match: spirit mana yang sudah capek dan perlu ke bench, dan kapan memakai Resonansi?

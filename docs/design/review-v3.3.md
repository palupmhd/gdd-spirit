# Review Desain Battle v3.3

Oct 18, 2026 · ditulis Claude atas permintaan Upi

Dokumen ini berisi masukan, bukan aturan baru. v3.3 tetap beku sesuai [status v3.3](v3.3-status.md). Semua usulan perubahan masuk ke [backlog hipotesis](hypothesis-backlog.md) dan baru diuji setelah prototipe dimainkan. Lubang spesifikasi yang harus diputuskan sebelum coding ada di [asumsi prototipe](prototype-assumptions.md).

## Kesimpulan

Fondasinya kuat. Format VGC, ekonomi ala Clash Royale, timeline ala Grandia, intro/outro ala Wuthering Waves, dan chain ala LoR/FaB belum pernah dijual sebagai satu paket. Diferensiasinya jelas.

Temuan terpenting dari review ini bisa dihitung di atas kertas, tanpa sim. Pertama, dengan asumsi setiap layer chain meniadakan layer di bawahnya, hasil chain penuh ditentukan oleh paritas jumlah spirit yang masih bisa menjawab. Ini menjelaskan tiga masalah sekaligus: pihak bertahan menang di chain penuh, penyerang sendirian 0 dari 4, dan 98% serangan masuk lewat celah timing. Kedua, Lelah berdurasi 4 detik selalu habis sebelum penyerang di 1v1 Ready lagi, jadi Lelah tidak bisa dihukum di duel. Ini menjelaskan duel 1v1 yang macet.

Dua temuan itu bukan alasan membuat v3.4 sekarang. Keduanya alasan untuk membangun prototipe yang resolver chain-nya bisa diganti, angka-angkanya ada di data, dan metriknya mencatat hal yang tepat. Dengan begitu playtest bisa mengonfirmasi atau membantah temuan ini dalam satu putaran.

Dari sisi pasar, ceruk "battle Pokémon doubles" sekarang dihuni Pokémon Champions, platform resmi VGC sejak 8 April 2026. Pemanggil Spirit tidak bisa menang dengan menjual dirinya sebagai alternatif Pokémon. Yang dijual harus hal yang tidak dimiliki Champions: real-time, chain aksi-reaksi, dan intro/outro. PvP live dari solo dev juga punya masalah populasi, jadi urutan rilis AI, async, lalu live di dokumen status sudah benar.

## 1. Ide awal, v3.3, dan referensinya

Hampir semua ide awal Upi sudah masuk ke v3.3. Tabel ini memetakan masing-masing ke game yang sudah membuktikan mekanik serupa.

| # | Ide awal | Bentuknya di v3.3 | Referensi | Catatan |
| --- | --- | --- | --- | --- |
| 1 | PvP | PvP, rilis bertahap AI, async, live | Backpack Battles, Super Auto Pets | Async PvP terbukti jalan untuk tim kecil (lihat bagian 5) |
| 2 | Bawa 6, main 4 | Bawa 6, ban 1, main 4 | Pokémon VGC | Format standar VGC. Ban 1 menambah lapisan draft |
| 3 | Arena 2v2 | 2v2 | VGC, Temtem (semua battle 2v2) | Format yang sudah dikenal pemain creature battle |
| 4 | Swap dengan intro/outro | Perfect Swap: Ready + 2 mana, intro dibatasi 1/3 Skill Utama | Wuthering Waves (Concerto, Intro/Outro) | Di WuWa, rotasi swap adalah optimasi APM untuk PvE. Di PvP, rotasi wajib berarti pajak eksekusi. v3.3 sudah benar menjadikannya keputusan dengan biaya |
| 5 | Auto battle yang paham role | 5 role, Energy, Skill Auto | Pokémon Unite (5 role), FFXII Gambit | Lihat bagian 3 |
| 6 | Mana global auto-fill | Mana bersama, +1 per 2 detik, maks 10, regen dobel di 4:00 | Clash Royale (elixir, double elixir di akhir) | Mana terlihat lawan, jadi bisa dihitung seperti pemain Clash Royale menghitung elixir. Cocok dengan pilar manajemen |
| 7-8 | Bar icon menentukan kapan boleh jalan | Timeline icon bersama, Ready di ujung | Grandia (IP gauge), Final Fantasy ATB | Grandia membolehkan cancel icon lawan. v3.3 sengaja melarang dorong/tarik icon lawan supaya Speed tidak jadi meta seperti Epic Seven dan Summoners War. Keputusan ini benar |
| 9 | Aksi-reaksi ala FaB dan spell speed LoR | Chain, tipe Kontak/Lontaran/Rapalan, Burst | LoR (Burst/Fast/Slow), Yu-Gi-Oh (spell speed, chain), MTG (stack), FaB (combat chain) | Burst v3.3 sama dengan Burst LoR: tidak bisa dijawab |

## 2. Temuan struktural

### 2.1 Chain penuh ditentukan paritas

Aturan v3.3: giliran bergantian mulai dari pihak bertahan, setiap spirit maksimal 1 layer, dan spirit yang memakai Skill Utama tidak ikut. Kalau setiap layer meniadakan layer tepat di bawahnya (dokumen status belum menyatakan ini, lihat asumsi A4), hasil chain cukup dihitung dari dua angka:

- d: jumlah spirit bertahan yang masih bisa menambah layer.
- a: jumlah spirit penyerang, selain si penyerang, yang masih bisa menambah layer.

Pihak yang kehabisan spirit lebih dulu kalah. Karena pihak bertahan bergerak duluan, pihak bertahan menang chain penuh kalau dan hanya kalau d > a.

| Situasi | d maks | a maks | Pemenang chain penuh | Cocok dengan data |
| --- | --- | --- | --- | --- |
| 2v2, semua siap | 2 | 1 | Bertahan (3 layer) | "Chain 3 layer selalu dimenangkan pihak bertahan" (v3.2) |
| 2v2, satu spirit bertahan tidak bisa menjawab | 1 | 1 | Penyerang | "98% serangan masuk lewat celah timing" (sim M1) |
| Penyerang sendirian, atau 1v1 | 1-2 | 0 | Bertahan, selalu | "Lucario sendirian 0 dari 4" (v3.3) |
| Bertahan sendirian melawan 2 | 1 | 1 | Penyerang | Belum diuji |

Satu-satunya pemecah paritas adalah Burst, karena Burst langsung menutup chain. Akibatnya Controller kemungkinan besar jadi pick wajib di setiap tim. Ini perlu dipantau sejak playtest pertama.

Konsekuensinya lebih besar dari sekadar "pihak bertahan terlalu kuat". Semua informasi yang menentukan d dan a terlihat: icon, mana, status Lelah, stun. Yang tersembunyi hanya loadout. Jadi penyerang yang rasional menghitung dulu, lalu hanya menyerang saat d ≤ a. Pihak bertahan yang rasional pass saat tahu akan kalah, supaya mana dan beban tersimpan. Chain penuh hanya terjadi karena kesalahan atau karena loadout tersembunyi, dan loadout tersembunyi adalah unsur tebakan yang justru ingin dihindari pilar manajemen.

Ini tidak otomatis buruk. Kalau dirangkul, game-nya menjadi "ciptakan celah": paksa lawan bereaksi supaya icon-nya mundur, kuras mana-nya, stun, lalu serang saat d ≤ a. Itu manajemen murni, mirip memanfaatkan frame advantage di fighting game. Masalahnya ada dua. Kalau celah muncul sendiri tanpa diciptakan, skill expression rendah. Dan slow-mo chain jadi seremoni untuk hasil yang sudah bisa dihitung, sehingga terasa memotong alur (pertanyaan playtest 2).

Cara mengujinya: untuk setiap Skill Utama yang mendarat, sim mencatat alasan spirit bertahan tidak menjawab. Kalau mayoritas alasannya "icon belum lewat garis tengah secara alami", celah terlalu lebar. Kalau mayoritas "icon mundur karena reaksi sebelumnya" atau "mana habis", celah memang diciptakan lewat permainan, dan itu tanda sehat.

Kalau playtest mengonfirmasi masalah ini, kandidat perbaikan terkuat adalah mengganti resolusi negasi dengan resolusi numerik ala FaB (hipotesis H3). Di FaB, hasil serangan adalah power dikurangi defense, dan kedua pihak memilih berapa banyak sumber daya yang dikorbankan. Hasilnya tidak bisa dihitung dari jumlah responder, tapi tetap tanpa tebakan.

### 2.2 Lelah tidak bisa dihukum di 1v1

Di 1v1, penyerang hanya punya satu sumber Ready. Setelah Skill Utama, icon-nya kembali ke awal dan butuh 8 sampai 11 detik untuk Ready lagi. Lelah muncul tepat setelah reaksi ketiga, yaitu tepat setelah penyerang memakai Ready-nya. Lelah hanya 4 detik, jadi selalu habis sebelum penyerang bisa menyerang lagi. Icon pihak bertahan memang kembali ke nol, tapi butuh hanya 4 sampai 5,5 detik untuk lewat garis tengah lagi, tetap lebih cepat dari Ready penyerang.

Di 2v2, partner penyerang bisa memukul selama 4 detik itu. Ini sebabnya Lelah bekerja di 2v2 (4 dari 4 dihukum) tapi tidak di duel. Anggapan v3.3 bahwa "Lelah mencegah bertahan tanpa batas di 1v1" gagal secara aritmetika.

Ada dua variabel yang memperparah atau memperbaiki ini, dan keduanya belum didefinisikan: apakah Ready boleh ditahan (asumsi A1), dan apakah jendela 20 detik bergulir atau reset setelah 20 detik tanpa reaksi (asumsi A2). Kalau jendelanya bergulir, spirit dengan siklus 11 detik hanya bisa menyerang dua kali dalam 20 detik, sehingga Lelah tidak pernah terpicu.

Obat yang sudah ada di daftar tertunda, chip damage saat beban tinggi, punya preseden kuat. Street Fighter 6 memakai Burnout: saat Drive Gauge habis, blok terkena chip damage (sekitar 25% menurut wiki komunitas) dan hanya dalam kondisi itu chip damage bisa KO. Itu mekanik anti-turtling yang sudah teruji di genre kompetitif terbesar untuk duel 1v1. Rekomendasinya: bangun chip damage sebagai toggle (mati secara default), supaya kalau playtest mengonfirmasi duel macet, perbaikannya cukup satu saklar.

### 2.3 Temuan lain

Damage otomatis sekitar 30%, di atas target 20 sampai 25%. Ini bukan sekadar soal angka. Setiap persen damage yang tidak lahir dari keputusan pemain adalah persen kekalahan yang bisa disalahkan ke game (bagian 4).

Intro Perfect Swap menghasilkan sekitar 10% damage bahkan pada bot acak. Artinya intro adalah nilai gratis, bukan keputusan. Pertahankan intro sebagai momen visual paling spektakuler (ini bahan trailer), tapi pertimbangkan intro yang memberi utilitas, bukan damage (hipotesis H9).

## 3. Role: dari 28 istilah ke 3 sumbu

Daftar di ide awal (Teamfight Roles, Engage, Peel, Diver, dan seterusnya) bukan 28 role. Itu campuran tiga sumbu yang berbeda, ditambah beberapa nama kategori.

| Sumbu | Istilah dari ide awal | Yang bermakna di 2v2 | Wujud di kode |
| --- | --- | --- | --- |
| Posisi | Frontline, Midline, Backline, Flank | Frontline, Backline, Flank. Midline hampir tidak bermakna dengan 2 spirit per sisi | Jarak preferensi dan sisi pendekatan |
| Fungsi tim | Engage, Initiator, Follow-up, Diver, Peel, Bodyguard, Counter-engage, Disengage, Zoner, Area Denial, Disruptor, Sustain, Support | Engage, Dive, Peel, Disengage, Zone | Primitif perilaku di utility AI |
| Profil damage | Damage Dealer, DPS, Sustained DPS, Burst, Finisher, Carry, Backline Threat | Sustained, Burst, Finisher | Pola auto-attack dan prioritas target |
| Bukan role | Teamfight Roles, Combat Roles, Combat Functions, Teamfight Responsibilities, Hero Role | Nama kategori | Tidak ada |

Lima role v3.3 adalah preset bobot di atas primitif tersebut:

| Role v3.3 | Posisi | Fungsi utama | Profil damage |
| --- | --- | --- | --- |
| Vanguard | Frontline | Engage, Bodyguard/Peel | Rendah, tahan lama |
| Striker | Backline | Follow-up | Sustained |
| Diver | Flank | Dive ke backline | Burst, Finisher |
| Controller | Backline | Zone, Disrupt, Counter-engage | Rendah, punya Burst |
| Support | Backline | Sustain, Peel, Disengage | Rendah |

Jumlah role yang dilihat pemain sebaiknya tetap kecil. Pokémon Unite memakai 5 role (Attacker, All-Rounder, Speedster, Defender, Supporter), dan Overwatch 2 malah memangkas menjadi 3 role saat pindah ke 5v5. Kosakata yang lebih kaya cukup hidup di dalam AI, tidak perlu jadi label di UI.

Aturan desain AI yang penting untuk PvP:

1. Bisa ditebak lebih penting dari pintar. Kekalahan karena AI berbuat aneh akan disalahkan ke game, bukan ke diri sendiri. Tie-break harus deterministik dan konsisten.
2. Niat AI harus terlihat: garis target dan ikon status perilaku (misalnya "Peel"), supaya pemain bisa membaca dan mengantisipasi.
3. Implementasi: utility AI dengan skor per primitif, dikalikan bobot role. Satu sistem, lima preset.
4. Gambit ala FFXII (pemain mengatur prioritas AI) menarik untuk kedalaman jangka panjang, tapi itu aturan baru. Masuk backlog dengan prioritas rendah.

## 4. Psikologi pemain

Agency. Self-determination theory menyebut otonomi dan kompetensi sebagai sumber motivasi intrinsik. Di game auto-battle, rasa "aku kalah karena keputusanku" adalah syarat pemain mau main lagi dan belajar. Target damage otomatis 20 sampai 25% sudah benar. Tambahkan ringkasan "titik balik" setelah match, misalnya "chain 2:31 kalah karena mana kurang 1". Sim deterministik membuat replay dan analisis seperti ini hampir gratis.

Beban kognitif. Perkiraan kasar: dua spirit sendiri Ready setiap 8 sampai 11 detik, jadi ada satu Ready setiap 4 sampai 5,5 detik. Dua spirit lawan menghasilkan peluang reaksi dengan ritme yang sama. Totalnya satu keputusan setiap 2 sampai 3 detik, sambil memantau 4 icon, 2 kolam mana, beban, dan Energy. Ritme keputusannya setara Clash Royale, tapi informasi yang harus dipantau jauh lebih banyak. UI harus hanya menonjolkan yang bisa ditindaklanjuti saat itu, sisanya bisa dilihat saat dibutuhkan. Ini inti pertanyaan playtest 5.

Kelelahan karena interupsi. MTG Arena punya auto-pass dan pengaturan stop karena priority stop yang terlalu sering melelahkan. LoR hanya menawarkan respons saat pemain punya respons yang sah. v3.3 sudah membuka chain hanya kalau ada reaksi sah. Langkah UI berikutnya, tanpa mengubah aturan: toggle auto-pass per spirit, dan layar chain yang langsung menunjukkan siapa yang masih bisa merespons dan sisa mana kedua pihak.

Optimasi membunuh fun. Soren Johnson: "Given the opportunity, players will optimize the fun out of a game." Kalau strategi optimal adalah menunggu celah, pemain akan menunggu, lalu bosan. Sim bot M1 sudah menunjukkan dua bot sabar tidak pernah menyerang. Metrik "jeda terpanjang tanpa aksi" wajib dicatat.

Tilt. Di 1v1 tidak ada rekan tim untuk disalahkan, jadi kekalahan terasa lebih personal. Penawarnya: match pendek (5 sampai 7 menit sudah tepat), surrender saat tertinggal 2 KO (sudah ada), antre ulang secepat mungkin, dan replay.

Kepercayaan. Stat disamakan, tanpa gacha, sekali beli. Di genre creature game yang penuh gacha, ini bukan hanya aturan balance tapi juga pesan pemasaran.

## 5. Pasar

| Game | Model | Data | Pelajaran |
| --- | --- | --- | --- |
| Pokémon Champions | Platform resmi VGC, Switch, mobile menyusul 2026 | Rilis 8 April 2026. Worlds 2026 memakai Champions | Ceruk battle doubles creature sudah dikuasai pemilik IP. Bersaing di real-time dan chain, bukan di "Pokémon versi lain" |
| Backpack Battles | Premium, async PvP, tim 2 orang | 500 ribu kopi dalam kurang dari 2 minggu, sekitar 640 ribu per April 2024, puncak sekitar 36 ribu CCU | Async PvP bisa jadi produk utama untuk tim sangat kecil |
| Mechabellum | Premium (USD 14,99 saat Early Access), PvP live auto-battler | Perkiraan pihak ketiga 435 sampai 575 ribu kopi, tapi puncak CCU hanya sekitar 3 sampai 5 ribu, sekitar 1 ribu pemain di 2026 | Bahkan PvP live premium yang sukses punya kolam pemain bersamaan yang kecil. Matchmaking live untuk solo dev akan tipis |
| Temtem | Creature MMO, semua battle 2v2 | Pengembangan berhenti 2024. Spin-off PvP Temtem Showdown ditutup karena hanya menarik pemain PvP yang sudah ada | Battle creature PvP saja tidak memperluas audiens |
| Legends of Runeterra | Card game PvP dengan spell speed | Januari 2024 PvP masuk mode pemeliharaan, ranked reward dan turnamen dibekukan, fokus ke PvE Path of Champions | Ada audiens yang suka interaksi spell speed tapi kehilangan rumah. Juga: PvE yang menopang LoR |
| Concord | Premium USD 40, hero shooter PvP | Puncak 697 CCU di Steam, ditutup 2 minggu setelah rilis | Skala berbeda, tapi pelajarannya sama: game PvP berbayar tanpa nilai single-player bisa mati sangat cepat |

Positioning yang disarankan: "battle doubles real-time di mana kamu tidak mengendalikan tangan spirit, tapi mengendalikan momen." Segmen audiens, dari yang paling dekat:

1. Pemain VGC dan Pokémon Showdown yang suka doubles tapi ingin tempo real-time.
2. Mantan pemain PvP LoR dan pemain FaB yang suka interaksi spell speed.
3. Pemain auto-battler (TFT, Mechabellum) yang ingin agency lebih besar saat combat.
4. Penggemar creature game (Palworld, Cassette Beasts, Temtem).

Strategi Steam: demo melawan AI untuk Steam Next Fest, karena itu mode yang selalu bisa dimainkan tanpa bergantung populasi. Hook trailer adalah intro/outro dan momen slow-mo chain. Replay deterministik bisa diekspor sebagai klip.

IP dan legal. Jangan pakai kata "Pokémon" di nama, kode, aset, atau materi publik. Nintendo dan The Pokémon Company menggugat Pocketpair (Palworld) atas paten pada September 2024. Game yang hanya berisi battle, tanpa mekanik tangkap, risikonya lebih kecil, tapi jaga jarak di nama, desain makhluk, dan UI. Ini bukan nasihat hukum. Kalau game mendekati rilis, konsultasikan ke ahli IP.

## 6. Prioritas

Urutan ini mengikuti langkah berikutnya di dokumen status, dengan tambahan dari review ini.

1. Upi mengunci pilar. Blok "pilar draft" di dokumen status tidak ikut terekspor (hanya tertulis "embedded content"), jadi isinya perlu ditulis ulang ke [pillars.md](pillars.md).
2. Upi memutuskan [asumsi prototipe](prototype-assumptions.md). Setiap baris sudah punya default, jadi cukup setuju atau koreksi.
3. Port core sim C# ke v3.3 dengan resolver chain yang bisa diganti, semua angka di data, hipotesis sebagai toggle yang mati secara default, dan metrik dari bagian 2.
4. Jalankan matriks bot lebih dulu. Murah, dan langsung menguji aritmetika di bagian 2.1 dan 2.2.
5. Bangun grey-box, playtest, lalu perbaiki satu masalah per putaran dari [backlog hipotesis](hypothesis-backlog.md).

## Sumber

- [Pokémon Champions jadi platform VGC (Nintendo Life)](https://www.nintendolife.com/news/2026/03/pokemon-vgc-competitions-officially-transition-to-pokemon-champions)
- [Tanggal rilis Pokémon Champions (Insider Gaming)](https://insider-gaming.com/pokemon-champions-release-date-announced/)
- [Backpack Battles 500 ribu kopi (GamingOnLinux)](https://gamingonlinux.com/2024/03/backpack-battles-has-sold-500000-copies-in-two-weeks-on-steam)
- [Backpack Battles 650 ribu kopi (GameDiscoverCo)](https://newsletter.gamediscover.co/p/how-backpack-battles-sold-650k-copies)
- [Backpack Battles CCU (Game World Observer)](https://gameworldobserver.com/2024/03/11/backpack-battles-sales-100k-copies-32k-concurrent-players)
- [Mechabellum Early Access (Paradox)](https://www.paradoxinteractive.com/media/press-releases/press-release/mechabellum-launches-into-early-access-today)
- [Mechabellum estimasi penjualan (Raijin)](https://raijin.gg/app/669330/Mechabellum) dan [SteamPulse](https://steampulse.org/game/669330)
- [Temtem akhir pengembangan (Kotaku)](https://kotaku.com/temtem-pokemon-mmo-end-development-1851310155)
- [Temtem Showdown ditutup (MMOs.com)](https://mmos.com/news/temtem-showdown-temcs-shutdown)
- [LoR 2024 (dotgg)](https://dotgg.gg/legends-of-the-runeterra-state-of-the-game-2024/) dan [outof.games](https://outof.games/news/7045-riot-details-the-future-of-legends-of-runeterra-one-more-set-path-of-champions-new-focus-more/)
- [Concord offline (PC Gamer)](https://www.pcgamer.com/games/fps/concord-is-being-taken-offline-this-week-as-sony-looks-to-explore-options-that-will-better-reach-our-players/) dan [puncak CCU (Dexerto)](https://www.dexerto.com/gaming/how-many-people-play-concord-2837752/)
- [SF6 Burnout (EventHubs)](https://eventhubs.com/news/2023/jun/01/what-to-do-burnout-sf6) dan [SF6 Gauges (SuperCombo wiki)](https://srk.shib.live/w/Street_Fighter_6/Gauges)
- [VGC 2023, open dan closed team sheets (Victory Road)](https://victoryroad.pro/2023-season/)

Data mekanik game (Clash Royale, Grandia, LoR, Yu-Gi-Oh, MTG, FaB, Wuthering Waves, Pokémon Unite, Overwatch 2, FFXII) berasal dari pengetahuan umum tentang game tersebut, bukan riset baru. Angka penjualan pihak ketiga adalah perkiraan.

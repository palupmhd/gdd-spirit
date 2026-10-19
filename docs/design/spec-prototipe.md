# Spec Prototipe

Status: berlaku (keputusan D6). Ini satu-satunya acuan aturan untuk kode. Disusun dari [v3.3](v3.3-status.md) dan [log keputusan](keputusan.md) D1 sampai D18. Semua angka adalah parameter awal di konfigurasi dan disetel lewat sim. Aturan hanya berubah lewat log keputusan, lalu dokumen ini diperbarui. Istilah game memakai bahasa Inggris (D11). Padanan nama lama ada di CLAUDE.md.

Setiap aturan dinilai terhadap [pilar](pillars.md): pelatih bukan pemain, manajemen terbuka, aksi-reaksi adalah jantung, naluri yang bisa dipelajari.

## 1. Scope putaran pertama

| Masuk | Belum masuk (toggle mati atau ditunda) |
| --- | --- |
| Timeline, Ready, Charge | Draft dan ban (tim tetap) |
| Mana bersama | Skill Area |
| Main Skill, tiga attack type, dan jangkauannya | Resonance |
| Arena top-down, jarak, dan Formation | Intro dan Outro |
| Chain dengan 18 reaction (D7, D12) | Recoverable Damage (D2) |
| Load dan Fatigue | Slow Zone |
| Emergency Swap, Perfect Swap tanpa Intro dan Outro, pengganti KO | Ghost async, PvP live, disconnect |
| Energy, Auto Skill, dan Instinct | Mode PvE dan akses roster (ditunda, H21 dan H22) |
| Overtime, menang di 3 KO (D3), surrender | Art, animasi, narasi |
| Lawan AI sederhana | |

Setiap tim membawa 4 spirit: 2 di arena dan 2 di bench.

## 2. Waktu

- 1 tick = 100 ms. Semua durasi dalam tick, semua persentase dalam basis point (10000 = 100%).
- Ada dua jam. Jam dunia menggerakkan timeline, gerak, auto-attack, Energy, mana, dan semua durasi status. Jam chain menghitung waktu keputusan di dalam chain.
- Selama chain, jam dunia beku total (D16). Arena hanya bergerak pelan sebagai efek visual, tanpa pengaruh ke logika. Tidak ada damage, regen, atau timer yang berjalan.
- Jam chain berjalan dalam tick waktu nyata, supaya keputusan dan batas waktu tetap deterministik untuk replay dan lockstep.
- Target durasi: 5 sampai 7 menit jam dunia, dan di bawah 10 menit waktu nyata (D16). Batas waktu nyata adalah target yang diukur, bukan aturan menang. Kalau sering terlampaui, angka yang disetel, bukan aturan baru.
- Batas match di sim adalah 12:00 jam dunia. Match yang mencapai batas ini dicatat sebagai macet.

## 3. Tim dan loadout

- Stat disamakan untuk PvP. HP awal 700, disetel lewat sim.
- Setiap spirit punya role, Speed, Instinct, pool 2 Main Skill, dan pool 3 reaction berisi 1 Basic Reaction dan 2 Special Reaction (D13).
- Loadout adalah semua pilihan yang dikunci sebelum antre: 1 dari 2 Main Skill per spirit, 2 dari 3 reaction per spirit, dan satu tipe Resonance per tim (D18).
- Press dimiliki semua spirit tanpa makan slot. Burst hanya ada di pool Controller.
- Pool bersifat publik. Loadout tersembunyi di ranked dan terbuka di turnamen best-of-3 (D5). AI dan ghost tidak boleh membaca loadout yang tersembunyi.

Contoh pool per role. Dua spirit dengan role sama dibedakan lewat satu Special Reaction.

| Role | Basic | Special | Varian |
| --- | --- | --- | --- |
| Vanguard | Shield | Intercept, Counter | Endure menggantikan Counter |
| Striker | Dodge | Pierce, Dominate | Retreat menggantikan Dominate |
| Diver | Dodge | Lock, Execute | Pressure menggantikan Execute |
| Controller | Interrupt | Cancel, Burst | Reflect atau Steal menggantikan Burst |
| Support | Shield | Heal, Retreat | Intercept atau Endure menggantikan Retreat |

## 4. Arena, jarak, dan Formation

- Arena 24 x 14 tile, dilihat dari atas (D9). Koordinat integer, 1 tile = 100 unit.
- Midline ada di x = 12 tile. Posisi depan dan belakang ditentukan oleh jarak ke midline saat itu, bukan slot tetap.
- Posisi awal: tim kiri di x = 4 tile, tim kanan di x = 20 tile, dua spirit berdampingan di y = 4 dan y = 10 tile.
- Gerak memakai steering sederhana (mendekat, menjauh, menjaga jarak) tanpa navmesh, karena arena terbuka tanpa rintangan.

Pita jarak (D14):

| Pita | Jarak |
| --- | --- |
| Self | Hanya spirit itu sendiri |
| Close | 2 tile |
| Mid | 7 tile |
| Global | Seluruh arena |

Jangkauan Main Skill:

| Attack type | Jangkauan |
| --- | --- |
| Contact | 6 tile |
| Projectile | 12 tile |
| Cast | 10 tile |

- Target Main Skill harus dalam jangkauan saat dideklarasikan. Kalau tidak ada target dalam jangkauan saat Ready, pemain bisa menahan Ready sambil AI mendekat, Charge, atau Perfect Swap.
- Semua jarak di chain diukur dari posisi saat chain dibuka.

Formation (D15):

| Formation | Perilaku AI |
| --- | --- |
| Compact | Kedua spirit menjaga jarak Mid satu sama lain |
| Balanced | Posisi mengikuti role dan Instinct. Default di awal match |
| Aggressive | Kedua spirit maju ke separuh arena lawan |

Formation berlaku untuk satu tim, terlihat lawan, bisa diganti kapan saja di luar chain, dengan jeda 30 tick jam dunia antar penggantian. Formation hanya mengubah arah gerak AI, tidak mengubah aturan resolusi.

## 5. Timeline

- Keempat spirit di arena berbagi satu timeline. Posisi ikon dari 0 sampai 10000.
- Panjang satu putaran 80 sampai 110 tick, linear dari Speed tertinggi ke terendah.
- Di awal match semua ikon di 5000 (midline timeline).
- Ikon di atas 5000 berarti spirit itu boleh ikut chain.
- Ikon di 10000 berarti Ready. Pemain memilih Main Skill, Charge, atau Perfect Swap. Ready boleh ditahan maksimal 30 tick, lalu otomatis Charge.
- Setelah aksi Ready, ikon kembali ke 0.
- Tidak ada efek yang bisa memajukan atau memundurkan ikon lawan.

## 6. Mana

- Satu kolam mana per pemain, terlihat lawan. Mulai 5, bertambah 1 setiap 20 tick, maksimal 10. Mana yang lewat batas terbuang.
- Charge: gratis, menambah mana sebesar panjang putaran spirit dalam detik dibagi 3,5, dibulatkan. Spirit lambat mendapat lebih banyak.
- Main Skill 3 mana, Perfect Swap 2 mana, Main Skill dari dalam Slow Zone +2 (belum masuk putaran pertama). Biaya reaction ada di bagian 8.

## 7. Main Skill dan attack type

- Power dasar Main Skill adalah 100.
- Pemain memilih target saat aktivasi, dari target yang dalam jangkauan. Default yang disarankan adalah target AI saat itu.

| Attack type | Cara mencapai target | Yang boleh menjawab | Basic Reaction yang cocok |
| --- | --- | --- | --- |
| Contact | Tubuh attacker bergerak ke target | Hanya target, kecuali lewat Intercept | Dodge |
| Projectile | Proyektil atau gelombang energi | Target atau partnernya | Shield |
| Cast | Efek muncul di target setelah dirapal | Target atau partnernya | Interrupt |

## 8. Reaction

Semua yang dipasang di chain disebut reaction. Basic Reaction hanya boleh dipakai kalau cocok attack type (D10). Special Reaction boleh dipakai apa pun attack type-nya. Kata target di tabel ini artinya target saat ini, yaitu target hasil resolusi semua reaction yang sudah dipasang (lihat bagian 9).

| Reaction | Jenis | Dipasang oleh | Biaya | Angka | Efek | Jarak |
| --- | --- | --- | --- | --- | --- | --- |
| Dodge | Basic | Defender | 2 | Block 60, hanya vs Contact | Tidak ada | Self |
| Shield | Basic | Defender | 2 | Block 60, hanya vs Projectile | Tidak ada | Self untuk target, Mid ke target untuk partner |
| Interrupt | Basic | Defender | 3 | Block 60, hanya vs Cast | Tidak ada | 10 tile ke attacker |
| Press | Universal | Attacker atau partner attacker | 2 | Power +30 | Tidak ada | Self untuk attacker, Mid ke target untuk partner |
| Intercept | Special | Partner target | 2 | Block 30 | Partner menjadi target baru. Boleh walau serangannya Contact | Mid ke target |
| Retreat | Special | Target | 2 | Block 0 | Target ditukar dengan spirit di bench, dan serangan mengenai spirit yang masuk. Butuh bench dan Emergency Swap tim tidak sedang jeda, lalu memasang jeda Emergency Swap | Self |
| Lock | Special | Attacker atau partner attacker | 2 | Power +10 | Semua perpindahan target di bawah reaction ini batal | Mid ke target |
| Counter | Special | Target | 2 | Block 40 | Kalau serangan berakhir Full Block dan attacker dalam jarak Close, attacker kena stun 10 tick setelah chain | Self |
| Reflect | Special | Defender | 2 | Block 20 | Kalau reaction tepat di bawahnya adalah Press, Press itu berbalik menjadi Power -30 | Self untuk target, Mid ke target untuk partner |
| Cancel | Special | Kedua pihak | 2 | Tidak ada | Membatalkan efek reaction tepat di bawahnya. Angkanya tetap | Mid ke spirit yang memasang reaction itu |
| Steal | Special | Kedua pihak | 3 | Block 20 kalau defender, Power +10 kalau attacker | Mengambil 1 mana dari kolam lawan. Kalau kolam lawan 0, tidak ada efek | Mid ke spirit lawan yang terlibat chain |
| Pressure | Special | Attacker atau partner attacker | 2 | Power +10 | Load target bertambah 1 | Close ke target |
| Dominate | Special | Attacker atau partner attacker | 2 | Power +10 | Setelah reaction ini, defender hanya boleh memasang 1 reaction lagi di chain ini. Berlaku saat dipasang dan tidak bisa di-Cancel | Self untuk attacker, Mid ke target untuk partner |
| Pierce | Special | Attacker atau partner attacker | 2 | 20 true damage | 20 damage tambahan yang tidak dikurangi Block. Serangan dengan Pierce tidak pernah Full Block | Self untuk attacker, Mid ke target untuk partner |
| Execute | Special | Attacker atau partner attacker | 3 | Power +10, atau +60 kalau HP target di bawah 25% saat chain dibuka | Tidak ada | Self untuk attacker, Mid ke target untuk partner |
| Endure | Special | Target | 2 | Block 20 | Chain ini tidak bisa membuat HP spirit itu turun di bawah 1 | Self |
| Heal | Special | Defender | 2 | Block 10 | Spirit yang menjadi target saat Heal dipasang pulih 30 HP setelah chain. Ikut turun di Overtime | Self untuk target, Mid ke target untuk partner |
| Burst | Special | Controller, kedua pihak | 4 | Block 60 kalau defender, Power +60 kalau attacker | Chain langsung selesai | Global |

## 9. Chain

### Kapan chain terbuka

1. Hanya ada satu chain pada satu waktu, dan Main Skill baru tidak bisa dideklarasikan selama chain berjalan (D16).
2. Kalau beberapa Main Skill dideklarasikan di tick yang sama, urutannya dari Speed tertinggi, lalu indeks slot. Main Skill berikutnya diperiksa ulang setelah chain sebelumnya selesai. Kalau attacker sudah KO atau kena stun, Main Skill itu batal tanpa membayar mana. Kalau targetnya sudah KO atau di luar jangkauan, pemain memilih target lain atau Ready tetap berjalan.
3. Mana Main Skill dibayar saat chain-nya dibuka.
4. Chain terbuka berdasarkan informasi publik saja (D16): ada minimal satu spirit defender yang boleh menjawab menurut attack type, ikonnya di atas 5000, tidak stun atau Fatigue, dalam jarak untuk minimal satu reaction di pool publiknya yang sah, dan mana defender cukup untuk reaction termurah yang memenuhi syarat itu. Kalau tidak ada, serangan langsung mendarat tanpa chain.

### Giliran

5. Defender bergerak pertama, lalu bergantian. Setiap giliran: pasang 1 reaction atau pass.
6. Satu spirit boleh memasang lebih dari satu reaction per chain, tapi setiap pihak hanya boleh memasang satu reaction bernama sama per chain (D8).
7. Syarat ikon dicek sekali saat chain dibuka. Si attacker boleh memasang reaction untuk serangannya sendiri walaupun ikonnya sudah di 0.
8. Kelayakan "hanya target" dan jarak ke target memakai target saat ini, yang ditampilkan di panel chain.
9. Chain selesai saat pihak yang dapat giliran pass, atau saat Burst dipasang.

### Waktu keputusan

10. Setiap giliran mendapat waktu dasar 40 tick waktu nyata yang tidak memotong bank (D16).
11. Setiap pemain punya bank 300 tick waktu nyata per match. Bank mulai berkurang setelah waktu dasar habis. Kalau bank kosong, giliran otomatis pass saat waktu dasar habis.
12. Pass otomatis, pass karena tidak punya pilihan, dan auto-pass yang diatur pemain selalu ditampilkan minimal 10 tick waktu nyata, supaya tidak bisa dibedakan dari pass manual.

### Resolusi (D7)

13. Efek reaction diselesaikan dari reaction teratas ke bawah.
14. Reaction yang efeknya dibatalkan tetap menyumbang angkanya.
15. Setelah semua efek beres, target akhir ditentukan.
16. Damage = Power total dikurangi Block total, minimal 10 (Chip Damage), ditambah true damage dari Pierce. Hasilnya dikali modifier (bagian 12), lalu dibatasi Endure kalau ada.
17. Full Block: kalau Block sama atau lebih besar dari Power, minimal satu Basic Reaction ikut dipakai, dan tidak ada Pierce, attacker kena Exposed (bagian 12). Efek tambahan Main Skill, seperti stun, hanya terjadi kalau serangan tidak Full Block. Buff attacker yang seharusnya terpakai tidak hilang kalau serangan Full Block.
18. Auto-pass: pemain bisa mengatur auto-pass per spirit. Ini pengaturan UX, bukan aturan.

## 10. Load dan Fatigue

- Setiap reaction yang dipasang menambah 1 load ke spirit yang memasangnya. Burst menambah 2. Pressure menambah 1 load ke target saat resolusi.
- Load kembali ke 0 setelah 200 tick jam dunia tanpa memasang reaction.
- Mundurnya ikon dihitung per reaction dan diterapkan setelah chain selesai: reaction yang membuat load jadi 1 memundurkan 5000, yang membuat load jadi 2 memundurkan 7500, minimal 0. Ikon si attacker tidak mundur.
- Reaction yang membuat load mencapai 3 boleh dipakai, tapi spirit itu tidak boleh memasang reaction lagi di chain tersebut. Load yang mencapai 3 dengan cara apa pun memicu Fatigue setelah chain selesai: ikon ke 0, selama 40 tick tidak bisa ikut chain, dan damage diterima +20%.
- Fatigue selesai saat durasinya habis, atau saat Main Skill spirit itu sendiri mendarat tanpa Full Block. Saat Fatigue selesai, load kembali ke 0.
- Spirit yang berada di bench selama 50 tick load-nya kembali ke 0, dan Fatigue-nya ikut hilang.

## 11. Swap, bench, dan pengganti KO

- Emergency Swap: hanya di luar chain, atau lewat Retreat di dalam chain. Spirit yang masuk mulai dari ikon 0 di posisi spirit yang keluar, tanpa Intro dan Outro. Setelah itu tim tidak bisa Emergency Swap lagi selama 50 tick.
- Perfect Swap: saat Ready, bayar 2 mana. Spirit yang masuk mulai di ikon 4000. Intro dan Outro mati di putaran pertama. Saat dinyalakan: damage Intro maksimal 1/3 Main Skill dan tidak bisa KO, Outro dengan buff sejenis tidak menumpuk, dan Outro bisa diarahkan ke spirit yang masuk atau partner di arena.
- Tim tanpa bench tidak bisa swap.
- Di bench, status berhenti berjalan dan HP tidak pulih. Recoverable Damage (D2) mati di putaran pertama. Saat dinyalakan: 40% dari setiap damage menjadi Recoverable Damage, dan setelah 50 tick di bench pulih 4% HP maksimal per detik sampai habis. Penurunan heal di Overtime ikut berlaku.
- Pengganti setelah KO: pemain memilih dalam 30 tick. Kalau tidak memilih, spirit pertama di urutan bench masuk. Pengganti masuk 30 tick setelah KO dengan ikon 7000, tanpa Intro dan tanpa jeda swap.

## 12. Status dan modifier

- Stun: tidak bisa bergerak, menyerang, atau ikut chain. Stun kedua dalam 60 tick durasinya separuh, stun ketiga tidak berefek. Tidak ada penalti damage untuk Main Skill ke target yang sedang stun (D16).
- Exposed: tanda yang dipasang oleh Full Block. Main Skill berikutnya ke spirit itu dalam 80 tick mendapat damage +30%, lalu tanda hilang. Exposed baru menyegarkan durasi, tidak menumpuk (D16).
- Exposed +30% dan Fatigue +20% dijumlahkan sebagai modifier damage diterima. Pengali Overtime dikalikan terpisah.

## 13. Role, Energy, Auto Skill, dan Instinct

- Lima role: Vanguard, Striker, Diver, Controller, Support.
- Energy terisi sesuai role: Vanguard dari damage yang diterima, Striker dan Diver dari damage yang diberikan, Controller dan Support seiring waktu. Laju disetel agar Auto Skill keluar rata-rata setiap 120 tick. Energy mulai di 50%.
- Saat Energy penuh, Auto Skill keluar sendiri: efek kecil atau damage kecil, tanpa chain, tidak bisa KO, dan tidak bisa dijawab. Titik awal damage Auto Skill: 5.
- Memakai Main Skill sendiri mengisi Energy 30%.
- Auto-attack titik awal: 1 damage setiap 15 tick. Jangkauan auto-attack: 1,5 tile untuk Vanguard dan Diver, 6 tile untuk Striker, Controller, dan Support. Target porsi damage otomatis adalah 20 sampai 25% dari total damage.
- Instinct (pilar 4): setiap spirit punya satu Instinct yang konsisten di atas perilaku role, misalnya selalu mengincar HP terendah atau selalu mengincar posisi belakang. Perilaku otomatis memakai utility AI: bobot role, ditambah Instinct, ditambah arah Formation, dengan tie-break deterministik. Niat AI (target dan perilaku aktif) selalu ditampilkan.
- Saat spirit Ready tapi tidak ada target dalam jangkauan Main Skill, AI memprioritaskan mendekat.

## 14. Pembukaan, Overtime, dan kemenangan

- Pembukaan: kedua lead masuk di 0:00 di posisi awal. Semua ikon di 5000, Energy 50%, mana 5, Formation Balanced.
- Overtime mulai 4:00 jam dunia: regen mana dua kali lipat. Mulai 4:30 dan setiap 30 detik setelahnya, damage naik 15% dan heal serta perisai HP turun 15%, menumpuk. Nilai Block dari reaction tidak terpengaruh.
- Menang saat lawan kehilangan 3 dari 4 spirit (D3). Tidak ada menang karena waktu.
- KO ketiga kedua tim di tick yang sama: tim dengan total persentase HP tersisa lebih tinggi menang. Kalau sama persis, seri.
- Surrender boleh kalau tertinggal 2 KO.

## 15. Resonance (belum masuk putaran pertama)

Sekali per match per tim, satu spirit yang Ready boleh beresonansi. Gratis dan tidak menghabiskan Ready. Sampai spirit itu KO, attack type Main Skill-nya berubah ke tipe Resonance tim, dan semua Basic Reaction-nya dianggap cocok tipe. Tipe Resonance adalah bagian dari loadout (D18), jadi tersembunyi di ranked dan terbuka di turnamen. Versi alternatif lewat syarat role ada di hipotesis H16.

## 16. Mode PvP (belum masuk putaran pertama)

- PvP live adalah ranked yang sesungguhnya (D17).
- PvP async adalah tantangan tim, bukan duel keterampilan chain. Ghost memakai instruksi yang disusun pemiliknya, misalnya kapan memakai Main Skill, reaction mana yang dipakai dalam situasi apa, dan Formation apa yang dipilih. Bentuk instruksinya dirancang setelah combat inti terbukti seru (hipotesis H11).

## 17. Toggle di konfigurasi

| Toggle | Default putaran pertama | Pilihan |
| --- | --- | --- |
| `chainResolver` | `Reduction` (D1) | `Negation` (model v3.3, hanya untuk pembanding di sim) |
| `recoverableDamage` | Mati | Hidup (D2) |
| `introOutro` | Mati | Hidup |
| `resonance` | Mati | Hidup |
| `slowZone` | Mati | Hidup |

## 18. Urutan putaran

1. Port sim, lalu jalankan matriks bot untuk kedua `chainResolver`. Ukur metrik wajib di CLAUDE.md.
2. Setel angka sampai match 5 sampai 7 menit jam dunia, di bawah 10 menit waktu nyata, dan porsi damage otomatis 20 sampai 25%.
3. Grey-box Unity dengan default putaran pertama, lalu playtest pertama oleh Upi.
4. Setiap putaran berikutnya menyalakan atau mengubah satu hal saja, berdasarkan catatan playtest.

## 19. Pertanyaan terbuka

Kalau ada aturan yang ambigu saat coding, tambahkan baris di sini dengan default yang dipakai, lalu sebutkan ke Upi.

| No | Pertanyaan | Jawaban atau default sementara |
| --- | --- | --- |
| 1 | Posisi depan dan belakang di arena top-down | Diputuskan (D9, D14): dari jarak ke midline saat itu |
| 2 | Reaction yang sama dua kali dalam satu chain | Diputuskan (D8): satu reaction bernama sama per pihak per chain |
| 3 | Apakah Fatigue ikut beku di bench? | Tidak. Fatigue hilang saat load direset setelah 50 tick di bench |
| 4 | Kenaikan damage pertama di Overtime | 4:30, lalu setiap 30 detik. Regen dua kali lipat mulai 4:00 |
| 5 | Tipe Resonance per tim atau per spirit | Satu tipe per tim, bagian dari loadout (D18) |
| 6 | Ready saat chain berjalan | Diputuskan (D16): jam dunia beku selama chain, jadi Ready tidak bisa tercapai di tengah chain |
| 7 | Dua Main Skill di tick yang sama | Diputuskan (D16): Speed tertinggi duluan, lalu indeks slot |
| 8 | Target durasi memakai jam apa | Diputuskan (D16): 5 sampai 7 menit jam dunia, di bawah 10 menit waktu nyata sebagai target |
| 9 | Spirit pengganti dari Retreat masuk ke chain yang sama? | Tidak. Spirit pengganti hanya menerima serangan dan tidak bisa memasang reaction di chain itu |
| 10 | Pressure pada spirit yang load-nya sudah 3 | Tidak ada efek tambahan. Fatigue tetap dipicu satu kali |
| 11 | Ikon mencapai Ready saat spirit sedang stun | Ready menunggu sampai stun selesai, dan timer menahan Ready tidak berjalan selama stun |
| 12 | Retreat yang efeknya batal (oleh Lock atau Cancel) | Pertukaran tidak terjadi, dan jeda Emergency Swap tim tidak terpasang |

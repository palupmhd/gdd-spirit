# Spec Prototipe

Status: berlaku (keputusan D6). Ini satu-satunya acuan aturan untuk kode. Disusun dari [v3.3](v3.3-status.md), [log keputusan](keputusan.md) D1 sampai D11, dan [asumsi prototipe](prototype-assumptions.md). Semua angka adalah parameter awal di konfigurasi dan disetel lewat sim. Aturan hanya berubah lewat log keputusan, lalu dokumen ini diperbarui. Istilah game memakai bahasa Inggris (D11). Padanan nama lama ada di CLAUDE.md.

Setiap aturan dinilai terhadap [pilar](pillars.md): pelatih bukan pemain, manajemen terbuka, aksi-reaksi adalah jantung, naluri yang bisa dipelajari.

## 1. Scope putaran pertama

Putaran playtest pertama dibuat sesempit mungkin supaya inti bisa diuji tanpa gangguan.

| Masuk | Belum masuk (toggle mati atau ditunda) |
| --- | --- |
| Timeline, Ready, Charge | Draft dan ban (tim tetap) |
| Mana bersama | Skill Area |
| Main Skill dan tiga attack type | Resonance |
| Chain model Block (D1) dengan Basic Reaction, Press, dan Burst | Intro dan Outro |
| Special Reaction (D7), setelah daftarnya disetujui | Recoverable Damage (D2) |
| Load dan Fatigue | Slow Zone |
| Emergency Swap, Perfect Swap tanpa Intro dan Outro, pengganti KO | Ghost async, PvP, disconnect |
| Energy dan Auto Skill | Art, animasi, narasi |
| Overtime, menang di 3 KO (D3), surrender | |
| Lawan AI sederhana | |

Setiap tim membawa 4 spirit: 2 di arena dan 2 di bench. Arena adalah lapangan 2D dilihat dari atas (D9).

## 2. Waktu

- 1 tick = 100 ms. Semua durasi dalam tick, semua persentase dalam basis point (10000 = 100%).
- Selama chain, dunia berjalan di 10%: ikon timeline, gerak, auto-attack, Energy, dan durasi status maju 1 tick untuk setiap 10 tick waktu nyata. Regen mana berhenti.
- Batas match di sim adalah 12:00. Match yang mencapai batas ini dicatat sebagai macet, bukan menang atau kalah.

## 3. Tim dan loadout

- Stat disamakan untuk PvP. HP awal 700, disetel lewat sim agar match berlangsung 5 sampai 7 menit.
- Setiap spirit punya role, Speed, 1 Main Skill (dipilih dari 2), dan 2 reaction (dipilih dari pool reaction spirit itu). Controller boleh mengisi salah satu slot dengan Burst. Model pool final menunggu keputusan di [diskusi 2026-10-19](diskusi-2026-10-19.md).
- Pool Main Skill dan reaction setiap spirit bersifat publik. Pilihan loadout tersembunyi di ranked dan terbuka di turnamen best-of-3 (D5). AI dan ghost tidak boleh membaca loadout yang tersembunyi.

## 4. Timeline

- Keempat spirit di arena berbagi satu timeline. Posisi ikon dari 0 sampai 10000.
- Panjang satu putaran 80 sampai 110 tick, linear dari Speed tertinggi ke terendah.
- Di awal match semua ikon di 5000 (midline).
- Ikon di atas 5000 berarti spirit itu boleh ikut chain.
- Ikon di 10000 berarti Ready. Pemain memilih Main Skill, Charge, atau Perfect Swap. Ready boleh ditahan maksimal 30 tick, lalu otomatis Charge.
- Setelah aksi Ready, ikon kembali ke 0.
- Tidak ada efek yang bisa memajukan atau memundurkan ikon lawan.

## 5. Mana

- Satu kolam mana per pemain, terlihat lawan. Mulai 5, bertambah 1 setiap 20 tick, maksimal 10. Mana yang lewat batas terbuang.
- Charge: gratis, menambah mana sebesar panjang putaran spirit dalam detik dibagi 3,5, dibulatkan. Spirit lambat mendapat lebih banyak.

| Aksi | Biaya |
| --- | --- |
| Main Skill | 3 |
| Dodge, Shield | 2 |
| Interrupt | 3 |
| Press | 2 |
| Burst | 4 |
| Perfect Swap | 2 |
| Main Skill dari dalam Slow Zone (belum masuk putaran pertama) | +2 |

## 6. Main Skill dan attack type

- Power dasar Main Skill adalah 100.
- Pemain memilih target saat aktivasi. Default yang disarankan adalah target AI saat itu.
- Main Skill ke target yang sedang stun damage-nya -30%.

| Attack type | Cara mencapai target | Yang boleh menjawab | Basic Reaction yang cocok |
| --- | --- | --- | --- |
| Contact | Tubuh attacker bergerak ke target | Hanya target | Dodge |
| Projectile | Proyektil atau gelombang energi | Target atau partnernya | Shield |
| Cast | Efek muncul di target setelah dirapal | Target atau partnernya | Interrupt |

## 7. Chain (D1)

Semua yang dipasang di chain disebut reaction. Special Reaction dengan efek dan resolusi efek dari atas (D7) menunggu persetujuan daftar reaction di [diskusi 2026-10-19](diskusi-2026-10-19.md). Sampai disetujui, bagian ini memakai Basic Reaction, Press, dan Burst.

### Reaction

| Reaction | Dipakai oleh | Nilai |
| --- | --- | --- |
| Dodge, Shield, Interrupt (Basic Reaction) | Defender | Block +60. Hanya boleh dipakai kalau cocok attack type (D10) |
| Press | Attacker: si attacker dan partnernya | Power +30. Dimiliki semua spirit, tidak butuh slot |
| Burst | Controller di pihak mana pun | Block +60 kalau bertahan atau Power +60 kalau menyerang, boleh melawan attack type apa pun, lalu chain langsung selesai |

### Aturan

1. Chain dibuka saat Main Skill dideklarasikan, kalau defender punya minimal satu reaction yang sah: spirit yang boleh menjawab sesuai attack type, ikon di atas 5000, tidak stun atau Fatigue, mana cukup, dan reaction-nya cocok tipe kalau itu Basic Reaction. Kalau tidak ada, serangan langsung mendarat tanpa chain.
2. Defender bergerak pertama, lalu bergantian. Setiap giliran: pasang 1 reaction atau pass.
3. Satu spirit boleh memasang lebih dari satu reaction per chain, tapi setiap pihak hanya boleh memasang satu reaction bernama sama per chain (D8). Syarat ikon dicek sekali saat chain dibuka.
4. Si attacker boleh memasang Press untuk serangannya sendiri walaupun ikonnya sudah di 0. Partner attacker butuh ikon di atas 5000 saat chain dibuka.
5. Chain selesai saat pihak yang dapat giliran pass, atau saat Burst dipasang.
6. Damage = Power total dikurangi Block total, minimal 10 (Chip Damage), lalu dikali modifier (bagian 11).
7. Full Block: kalau Block sama atau lebih besar dari Power dan minimal satu Basic Reaction yang cocok tipe ikut dipakai, attacker jadi Vulnerable selama 20 tick (damage diterima +30%). Efek tambahan Main Skill, seperti stun, hanya terjadi kalau serangan tidak Full Block. Buff attacker yang seharusnya terpakai tidak hilang kalau serangan Full Block.
8. Setiap pemain punya bank waktu 35 detik waktu nyata per match. Kalau habis, setiap giliran hanya 0,5 detik, tapi defender selalu mendapat minimal 1 detik.
9. Auto-pass: pemain bisa mengatur auto-pass per spirit. Ini pengaturan UX, bukan aturan.

## 8. Load dan Fatigue

- Setiap reaction yang dipasang menambah 1 load ke spirit yang memasangnya. Burst menambah 2.
- Load kembali ke 0 setelah 200 tick tanpa memasang reaction.
- Mundurnya ikon dihitung per reaction dan diterapkan setelah chain selesai: reaction yang membuat load jadi 1 memundurkan 5000, yang membuat load jadi 2 memundurkan 7500, minimal 0. Ikon si attacker tidak mundur.
- Reaction yang membuat load mencapai 3 boleh dipakai, tapi spirit itu tidak boleh memasang reaction lagi di chain tersebut. Setelah chain selesai, spirit itu kena Fatigue: ikon ke 0, selama 40 tick tidak bisa ikut chain, dan damage diterima +20%.
- Fatigue selesai saat durasinya habis, atau saat Main Skill spirit itu sendiri mendarat tanpa Full Block. Saat Fatigue selesai, load kembali ke 0.
- Spirit yang berada di bench selama 50 tick load-nya kembali ke 0.

## 9. Swap, bench, dan pengganti KO

- Emergency Swap: hanya di luar chain. Spirit yang masuk mulai dari ikon 0, tanpa Intro dan Outro. Setelah itu tim tidak bisa Emergency Swap lagi selama 50 tick.
- Perfect Swap: saat Ready, bayar 2 mana. Spirit yang masuk mulai di ikon 4000. Intro dan Outro mati di putaran pertama. Saat dinyalakan: damage Intro maksimal 1/3 Main Skill dan tidak bisa KO, Outro dengan buff sejenis tidak menumpuk, dan Outro bisa diarahkan ke spirit yang masuk atau partner di arena.
- Tim tanpa bench tidak bisa swap.
- Di bench, status berhenti berjalan dan HP tidak pulih. Recoverable Damage (D2) mati di putaran pertama. Saat dinyalakan: 40% dari setiap damage menjadi Recoverable Damage, dan setelah 50 tick di bench pulih 4% HP maksimal per detik sampai habis. Penurunan heal di Overtime ikut berlaku.
- Pengganti setelah KO: pemain memilih dalam 30 tick. Kalau tidak memilih, spirit pertama di urutan bench masuk. Pengganti masuk 30 tick setelah KO dengan ikon 7000, tanpa Intro dan tanpa cooldown swap.

## 10. Role, Energy, Auto Skill, dan Instinct

- Lima role: Vanguard, Striker, Diver, Controller, Support.
- Energy terisi sesuai role: Vanguard dari damage yang diterima, Striker dan Diver dari damage yang diberikan, Controller dan Support seiring waktu. Laju disetel agar Auto Skill keluar rata-rata setiap 120 tick. Energy mulai di 50%.
- Saat Energy penuh, Auto Skill keluar sendiri: efek kecil atau damage kecil, tanpa chain, tidak bisa KO, dan tidak bisa dijawab. Titik awal damage Auto Skill: 5.
- Memakai Main Skill sendiri mengisi Energy 30%.
- Auto-attack titik awal: 1 damage setiap 15 tick. Target porsi damage otomatis (auto-attack dan Auto Skill) adalah 20 sampai 25% dari total damage. Titik awal ini hitungan kertas dan pasti disetel ulang lewat sim.
- Instinct (pilar 4): setiap spirit punya satu Instinct yang konsisten di atas perilaku role, misalnya selalu mengincar HP terendah atau selalu mengincar posisi belakang. Perilaku otomatis memakai utility AI dengan bobot per role ditambah Instinct, dengan tie-break deterministik. Niat AI (target dan perilaku aktif) selalu ditampilkan.
- Gerak di arena top-down memakai koordinat integer dan steering sederhana (mendekat, menjauh, menjaga jarak), tanpa navmesh.

## 11. Status dan modifier

- Stun: tidak bisa bergerak, menyerang, atau ikut chain. Stun kedua dalam 60 tick durasinya separuh, stun ketiga tidak berefek.
- Vulnerable +30% dan Fatigue +20% dijumlahkan sebagai modifier damage diterima. Pengali Overtime dikalikan terpisah.
- Penskalaan damage di chain ala FaB dan BlazBlue dari v3.3 digantikan model Block.

## 12. Pembukaan, Overtime, dan kemenangan

- Pembukaan: kedua lead masuk di 0:00. Semua ikon di 5000, Energy 50%, mana 5.
- Overtime mulai 4:00: regen mana dua kali lipat. Mulai 4:30 dan setiap 30 detik setelahnya, damage naik 15% dan heal serta perisai HP turun 15%, menumpuk. Nilai Block dari reaction tidak terpengaruh.
- Menang saat lawan kehilangan 3 dari 4 spirit (D3). Tidak ada menang karena waktu.
- KO ketiga kedua tim di tick yang sama: tim dengan total persentase HP tersisa lebih tinggi menang. Kalau sama persis, seri.
- Surrender boleh kalau tertinggal 2 KO.

## 13. Resonance (belum masuk putaran pertama)

Versi v3.3 yang diterjemahkan ke model D1: sekali per match per tim, satu spirit yang Ready boleh beresonansi. Gratis dan tidak menghabiskan Ready. Sampai spirit itu KO, attack type Main Skill-nya berubah ke tipe Resonance tim, dan semua Basic Reaction-nya dihitung cocok tipe. Tipe Resonance dipilih satu per tim di team builder, tersembunyi di ranked dan terbuka di turnamen. Versi alternatif lewat syarat role ada di hipotesis H16.

## 14. Toggle di konfigurasi

| Toggle | Default putaran pertama | Pilihan |
| --- | --- | --- |
| `chainResolver` | `Reduction` (D1) | `Negation` (model v3.3, hanya untuk pembanding di sim) |
| `recoverableDamage` | Mati | Hidup (D2) |
| `introOutro` | Mati | Hidup |
| `resonance` | Mati | Hidup |
| `slowZone` | Mati | Hidup |

## 15. Urutan putaran

1. Port sim, lalu jalankan matriks bot untuk kedua `chainResolver`. Ukur metrik wajib di CLAUDE.md, terutama chain per menit, prediktabilitas chain, dan porsi damage otomatis.
2. Setel angka sampai match 5 sampai 7 menit dan porsi damage otomatis 20 sampai 25%.
3. Grey-box Unity dengan default putaran pertama, lalu playtest pertama oleh Upi.
4. Setiap putaran berikutnya menyalakan atau mengubah satu hal saja, berdasarkan catatan playtest.

## 16. Pertanyaan terbuka

Kalau ada aturan yang ambigu saat coding, tambahkan baris di sini dengan default yang dipakai, lalu sebutkan ke Upi.

| No | Pertanyaan | Default sementara |
| --- | --- | --- |
| 1 | Bagaimana posisi depan dan belakang ditentukan di arena top-down (D9), dan apa artinya untuk target Contact? | Posisi depan dan belakang dihitung dari jarak ke midline saat itu, bukan slot tetap. Posisi awal: dua spirit berdampingan atas-bawah. Contact boleh menargetkan siapa saja |
| 2 | Bolehkah reaction yang sama dipakai dua kali dalam satu chain? | Diputuskan (D8): setiap pihak hanya boleh memasang satu reaction bernama sama per chain |
| 3 | Fatigue adalah status, dan status berhenti di bench. Apakah Fatigue ikut beku di bench? | Tidak. Fatigue hilang saat load direset setelah 50 tick di bench, karena bench adalah obat load |
| 4 | Kenaikan damage pertama di Overtime terjadi di 4:00 atau 4:30? | 4:30, lalu setiap 30 detik setelahnya. Regen mana dua kali lipat sudah mulai di 4:00 |
| 5 | Tipe Resonance dipilih per tim atau per spirit? | Satu tipe per tim, dipakai oleh spirit mana pun yang beresonansi |
| 6 | Apa yang terjadi kalau ikon mencapai Ready saat chain berjalan? | Ready menunggu sampai chain selesai, dan timer menahan Ready tidak berjalan selama chain. Usulan perubahan aturan ada di audit 2026-10-19, P0 nomor 1 dan 2 |
| 7 | Bagaimana urutan dua Main Skill yang dideklarasikan di tick yang sama? | Speed tertinggi duluan, lalu indeks slot. Main Skill kedua membuka chain baru setelah chain pertama selesai |
| 8 | Target 5 sampai 7 menit memakai waktu game atau waktu nyata? | Waktu game. Waktu nyata dicatat terpisah di sim dan playtest. Usulan batas waktu nyata ada di audit 2026-10-19, P0 nomor 5 |

# Spec Prototipe

Status: berlaku (keputusan D6). Ini satu-satunya acuan aturan untuk kode. Disusun dari [v3.3](v3.3-status.md), [log keputusan](keputusan.md) D1 sampai D6, dan [asumsi prototipe](prototype-assumptions.md). Semua angka adalah parameter awal di konfigurasi dan disetel lewat sim. Aturan hanya berubah lewat log keputusan, lalu dokumen ini diperbarui.

Setiap aturan dinilai terhadap [pilar](pillars.md): pelatih bukan pemain, manajemen terbuka, aksi-reaksi adalah jantung, naluri yang bisa dipelajari.

## 1. Scope putaran pertama

Putaran playtest pertama dibuat sesempit mungkin supaya inti bisa diuji tanpa gangguan.

| Masuk | Belum masuk (toggle mati atau ditunda) |
| --- | --- |
| Timeline icon, Ready, Charge | Draft dan ban (tim tetap) |
| Mana bersama | Skill Area |
| Skill Utama dan tiga tipe serangan | Resonansi |
| Chain model pengurangan damage (D1), mode izin tipe | Tipe sebagai efisiensi (H17) |
| Beban dan Lelah | Efek intro dan outro |
| Swap Darurat, Perfect Swap tanpa efek intro/outro, pengganti KO | Luka ringan (D2) |
| Energy dan Skill Auto | Zona slow |
| Fase Akhir, menang di 3 KO (D3), surrender | Ghost async, PvP, disconnect |
| Lawan AI sederhana | Art, animasi, narasi |

Setiap tim membawa 4 spirit: 2 di arena dan 2 di bench.

## 2. Waktu

- 1 tick = 100 ms. Semua durasi dalam tick, semua persentase dalam basis point (10000 = 100%).
- Selama chain, dunia berjalan di 10%: icon, gerak, auto-attack, Energy, dan durasi status maju 1 tick untuk setiap 10 tick waktu nyata. Regen mana berhenti.
- Batas match di sim adalah 12:00. Match yang mencapai batas ini dicatat sebagai macet, bukan menang atau kalah.

## 3. Tim dan loadout

- Stat disamakan untuk PvP. HP awal 700, disetel lewat sim agar match berlangsung 5 sampai 7 menit.
- Setiap spirit punya role, Speed, 1 Skill Utama (dipilih dari 2), dan 2 tangkisan (dipilih dari Dodge, Shield, Interrupt). Controller boleh mengisi salah satu slot dengan Burst.
- Pool Skill Utama dan tangkisan setiap spirit bersifat publik. Pilihan loadout tersembunyi di ranked dan terbuka di turnamen best-of-3 (D5). AI dan ghost tidak boleh membaca loadout yang tersembunyi.

## 4. Timeline icon

- Keempat spirit di arena berbagi satu timeline. Posisi icon dari 0 sampai 10000.
- Panjang satu putaran 80 sampai 110 tick, linear dari Speed tertinggi ke terendah.
- Di awal match semua icon di 5000 (garis tengah).
- Icon di atas 5000 berarti spirit itu boleh ikut chain.
- Icon di 10000 berarti Ready. Pemain memilih Skill Utama, Charge, atau Perfect Swap. Ready boleh ditahan maksimal 30 tick, lalu otomatis Charge.
- Setelah aksi Ready, icon kembali ke 0.
- Tidak ada efek yang bisa memajukan atau memundurkan icon lawan.

## 5. Mana

- Satu kolam mana per pemain, terlihat lawan. Mulai 5, bertambah 1 setiap 20 tick, maksimal 10. Mana yang lewat batas terbuang.
- Charge: gratis, menambah mana sebesar panjang putaran spirit dalam detik dibagi 3,5, dibulatkan. Spirit lambat mendapat lebih banyak.

| Aksi | Biaya |
| --- | --- |
| Skill Utama | 3 |
| Dodge, Shield | 2 |
| Interrupt | 3 |
| Tekan | 2 |
| Burst | 4 |
| Perfect Swap | 2 |
| Skill Utama dari dalam zona slow (belum masuk putaran pertama) | +2 |

## 6. Skill Utama dan tipe serangan

- Power dasar Skill Utama adalah 100.
- Pemain memilih target saat aktivasi. Default yang disarankan adalah target AI saat itu.
- Skill Utama ke target yang sedang stun damage-nya -30%.

| Tipe | Cara mencapai target | Yang boleh menangkis | Tangkisan cocok |
| --- | --- | --- | --- |
| Kontak | Tubuh penyerang bergerak ke target | Hanya target | Dodge |
| Lontaran | Proyektil atau gelombang energi | Target atau partnernya | Shield |
| Rapalan | Efek muncul di target setelah dirapal | Target atau partnernya | Interrupt |

## 7. Chain (D1)

### Kartu

| Kartu | Dipakai oleh | Nilai |
| --- | --- | --- |
| Dodge, Shield, Interrupt | Pihak bertahan | Tahan +60 kalau cocok tipe. Di mode izin, kartu yang tidak cocok tidak boleh dipakai. Di mode efisiensi (H17), kartu yang tidak cocok bernilai Tahan +30 |
| Tekan | Pihak penyerang: si penyerang dan partnernya | Power +30. Dimiliki semua spirit, tidak butuh slot |
| Burst | Controller di pihak mana pun | Tahan +60 kalau bertahan atau Power +60 kalau menyerang, boleh melawan tipe apa pun, lalu chain langsung selesai |

### Aturan

1. Chain dibuka saat Skill Utama dideklarasikan, kalau pihak bertahan punya minimal satu kartu yang boleh dipakai: spirit yang boleh menangkis sesuai tipe, icon di atas 5000, tidak stun atau Lelah, mana cukup, dan kartunya sah menurut mode tipe. Kalau tidak ada, serangan langsung mendarat tanpa chain.
2. Pihak bertahan bergerak pertama, lalu bergantian. Setiap giliran: taruh 1 kartu atau pass.
3. Satu spirit boleh menaruh lebih dari satu kartu per chain. Syarat icon dicek sekali saat chain dibuka.
4. Si penyerang boleh menaruh Tekan untuk serangannya sendiri walaupun icon-nya sudah di 0. Partner penyerang butuh icon di atas 5000 saat chain dibuka.
5. Chain selesai saat pihak yang dapat giliran pass, atau saat Burst dipasang.
6. Damage = Power total dikurangi Tahan total, minimal 10 (chip damage), lalu dikali modifier (bagian 11).
7. Tertahan penuh: kalau Tahan sama atau lebih besar dari Power dan minimal satu kartu cocok tipe ikut dipakai, penyerang jadi Vulnerable selama 20 tick (damage diterima +30%). Efek tambahan Skill Utama, seperti stun, hanya terjadi kalau serangan tidak tertahan penuh. Buff penyerang yang seharusnya terpakai tidak hilang kalau serangan tertahan penuh.
8. Setiap pemain punya bank waktu 35 detik waktu nyata per match. Kalau habis, setiap giliran hanya 0,5 detik, tapi pihak bertahan selalu mendapat minimal 1 detik.
9. Auto-pass: pemain bisa mengatur auto-pass per spirit. Ini pengaturan UX, bukan aturan.

## 8. Beban dan Lelah

- Setiap kartu yang ditaruh menambah 1 beban ke spirit yang menaruhnya. Burst menambah 2.
- Beban kembali ke 0 setelah 200 tick tanpa menaruh kartu.
- Mundurnya icon dihitung per kartu dan diterapkan setelah chain selesai: kartu dengan beban 1 memundurkan 5000, kartu dengan beban 2 memundurkan 7500, minimal 0. Icon si penyerang tidak mundur.
- Kartu yang membuat beban mencapai 3 boleh dipakai, tapi spirit itu tidak boleh menaruh kartu lagi di chain tersebut. Setelah chain selesai, spirit itu kena Lelah: icon ke 0, selama 40 tick tidak bisa ikut chain, dan damage diterima +20%.
- Lelah selesai saat durasinya habis, atau saat Skill Utama spirit itu sendiri mendarat tanpa tertahan penuh. Saat Lelah selesai, beban kembali ke 0.
- Spirit yang berada di bench selama 50 tick bebannya kembali ke 0.

## 9. Swap, bench, dan pengganti KO

- Swap Darurat: hanya di luar chain. Spirit yang masuk mulai dari icon 0, tanpa intro dan outro. Setelah itu tim tidak bisa Swap Darurat lagi selama 50 tick.
- Perfect Swap: saat Ready, bayar 2 mana. Spirit yang masuk mulai di icon 4000. Efek intro dan outro mati di putaran pertama. Saat dinyalakan: damage intro maksimal 1/3 Skill Utama dan tidak bisa KO, outro dengan buff sejenis tidak menumpuk, dan outro bisa diarahkan ke spirit yang masuk atau partner di arena.
- Tim tanpa bench tidak bisa swap.
- Di bench, status berhenti berjalan dan HP tidak pulih. Luka ringan (D2) mati di putaran pertama. Saat dinyalakan: 40% dari setiap damage menjadi luka ringan, dan setelah 50 tick di bench luka ringan pulih 4% HP maksimal per detik sampai habis. Penurunan heal di Fase Akhir ikut berlaku.
- Pengganti setelah KO: pemain memilih dalam 30 tick. Kalau tidak memilih, spirit pertama di urutan bench masuk. Pengganti masuk 30 tick setelah KO dengan icon 7000, tanpa intro dan tanpa cooldown swap.

## 10. Role, Energy, Skill Auto, dan naluri

- Lima role: Vanguard, Striker, Diver, Controller, Support.
- Energy terisi sesuai role: Vanguard dari damage yang diterima, Striker dan Diver dari damage yang diberikan, Controller dan Support seiring waktu. Laju disetel agar Skill Auto keluar rata-rata setiap 120 tick. Energy mulai di 50%.
- Saat Energy penuh, Skill Auto keluar sendiri: efek kecil atau damage kecil, tanpa chain, tidak bisa KO, dan tidak bisa dijawab. Titik awal damage Skill Auto: 5.
- Memakai Skill Utama sendiri mengisi Energy 30%.
- Auto-attack titik awal: 1 damage setiap 15 tick. Target porsi damage otomatis (auto-attack dan Skill Auto) adalah 20 sampai 25% dari total damage. Titik awal ini hitungan kertas dan pasti disetel ulang lewat sim.
- Naluri (pilar 4): setiap spirit punya satu naluri yang konsisten di atas perilaku role, misalnya selalu mengincar HP terendah atau selalu mengincar posisi belakang. Perilaku otomatis memakai utility AI dengan bobot per role ditambah naluri, dengan tie-break deterministik. Niat AI (target dan perilaku aktif) selalu ditampilkan.

## 11. Status dan modifier

- Stun: tidak bisa bergerak, menyerang, atau ikut chain. Stun kedua dalam 60 tick durasinya separuh, stun ketiga tidak berefek.
- Vulnerable +30% dan Lelah +20% dijumlahkan sebagai modifier damage diterima. Pengali Fase Akhir dikalikan terpisah.
- Penskalaan damage di chain ala FaB dan BlazBlue dari v3.3 digantikan model pengurangan damage.

## 12. Pembukaan, Fase Akhir, dan kemenangan

- Pembukaan: kedua lead masuk di 0:00. Semua icon di 5000, Energy 50%, mana 5.
- Fase Akhir mulai 4:00: regen mana dua kali lipat. Setiap 30 detik, damage naik 15% dan heal serta perisai HP turun 15%, menumpuk. Nilai Tahan dari kartu tidak terpengaruh.
- Menang saat lawan kehilangan 3 dari 4 spirit (D3). Tidak ada menang karena waktu.
- KO ketiga kedua tim di tick yang sama: tim dengan total persentase HP tersisa lebih tinggi menang. Kalau sama persis, seri.
- Surrender boleh kalau tertinggal 2 KO.

## 13. Resonansi (belum masuk putaran pertama)

Versi v3.3 yang diterjemahkan ke model D1: sekali per match per tim, satu spirit yang Ready boleh beresonansi. Gratis dan tidak menghabiskan Ready. Sampai spirit itu KO, tipe Skill Utama-nya berubah ke tipe Resonansi yang dipilih di team builder, dan semua tangkisannya dihitung cocok tipe. Tipe Resonansi tersembunyi di ranked dan terbuka di turnamen. Versi alternatif lewat syarat role ada di hipotesis H16.

## 14. Toggle di konfigurasi

| Toggle | Default putaran pertama | Pilihan |
| --- | --- | --- |
| `chainResolver` | `Reduction` (D1) | `Negation` (model v3.3, hanya untuk pembanding di sim) |
| `typeMatchMode` | `Legality` | `Efficiency` (H17) |
| `recoverableDamage` | Mati | Hidup (D2) |
| `introOutro` | Mati | Hidup |
| `resonance` | Mati | Hidup |
| `slowZone` | Mati | Hidup |

## 15. Urutan putaran

1. Port sim, lalu jalankan matriks bot untuk kedua `typeMatchMode` dan kedua `chainResolver`. Ukur metrik wajib di CLAUDE.md, terutama chain per menit, prediktabilitas chain, dan porsi damage otomatis.
2. Setel angka sampai match 5 sampai 7 menit dan porsi damage otomatis 20 sampai 25%.
3. Grey-box Unity dengan default putaran pertama, lalu playtest pertama oleh Upi.
4. Setiap putaran berikutnya menyalakan atau mengubah satu hal saja, berdasarkan catatan playtest.

## 16. Pertanyaan terbuka

Kalau ada aturan yang ambigu saat coding, tambahkan baris di sini dengan default yang dipakai, lalu sebutkan ke Upi.

| No | Pertanyaan | Default sementara |
| --- | --- | --- |
| 1 | Bagaimana posisi depan dan belakang ditentukan di arena 2D samping, dan apa artinya untuk target Kontak? | Spirit yang lebih dekat ke tengah adalah posisi depan. Kontak boleh menargetkan siapa saja |

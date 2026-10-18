# Backlog Hipotesis

Semua usulan perubahan aturan masuk ke sini, bukan langsung ke versi baru. Setiap hipotesis harus menyebut masalah yang diobati, preseden dari game lain, dan metrik yang membuktikan berhasil atau gagal. Di prototipe, hipotesis boleh dibangun sebagai toggle yang mati secara default. Default hanya berubah setelah playtest menunjukkan masalahnya nyata, satu hipotesis per putaran.

Urutan di tabel adalah urutan uji yang disarankan, dari yang paling murah dan paling didukung bukti.

| ID | Masalah | Perubahan | Preseden | Lolos kalau | Status |
| --- | --- | --- | --- | --- | --- |
| H1 | Duel 1v1 macet | Chip damage 25% dari Skill Utama yang di-counter saat beban reaction 2 atau lebih. Chip bisa KO | Street Fighter 6, Burnout | Duel 1v1 selesai di bawah 45 detik, tanpa membuat 2v2 lebih pendek dari 5 menit | Tertunda (dari dokumen status) |
| H2 | Duel 1v1 macet | Lelah berlangsung sampai icon spirit itu sendiri lewat garis tengah lagi, bukan 4 detik | Turunan dari review 2.2 | Lelah di 1v1 dihukum minimal 1 dari 2 kali | Alternatif H1, uji hanya kalau H1 gagal |
| H3 | Pihak bertahan menang chain penuh, chain bisa dihitung sebelum mulai, celah timing | Resolusi numerik: Skill Utama punya power, reaction mengurangi damage sesuai mana yang dikeluarkan, penyerang boleh menambah power dengan mana. Hasil tidak lagi biner | Flesh and Blood (power dikurangi defense) | Pemenang chain penuh tidak bisa ditebak dari jumlah responder saja, dan porsi serangan yang masuk lewat celah alami turun di bawah 50% | Uji di sim bot dulu, perubahan terbesar |
| H4 | Pihak bertahan menang chain penuh | Spirit penyerang boleh memasang satu attack reaction di chain-nya sendiri | Flesh and Blood (attack reaction) | Pemenang chain penuh 2v2 seimbang 40 sampai 60% | Alternatif H3 yang lebih kecil |
| H5 | Pihak bertahan menumpuk layer | Setiap layer tambahan dari pihak yang sama +1 mana | Pajak eskalasi, umum di card game | Chain 3 layer turun tanpa membuat reaksi pertama jarang | Ide |
| H6 | Dua pemain sabar macet | Batas menahan Ready lebih ketat dari default A1 | v1 dan v3.5 (diarsipkan) | Jeda terpanjang tanpa aksi di bawah 15 detik | Hanya kalau default A1 kurang |
| H7 | Lelah terlalu menghukum di 2v2 | Bonus damage Lelah dari +20% ke +10% | Dari dokumen status | Spirit yang kena Lelah tidak otomatis KO dalam 4 detik | Tertunda (dari dokumen status) |
| H8 | Lubang tipe sekitar 30% karena bawa 2 dari 3 reaction | Loadout terbuka di semua mode, atau bawa 3 dari 3 | Pokémon VGC (open team sheet di banyak turnamen resmi) | Serangan yang masuk karena lubang tipe di bawah 15% | Ide, bersinggungan dengan pilar manajemen |
| H9 | Intro jadi sumber damage gratis | Intro memberi utilitas (shield kecil, Energy, cleanse), bukan damage | Wuthering Waves memakai intro sebagai pemicu rotasi | Damage dari intro di bawah 5% total damage | Ide |
| H10 | Damage otomatis terlalu besar | Turunkan damage auto-attack sampai porsinya 20 sampai 25% | Target di dokumen status | Porsi damage otomatis 20 sampai 25% | Penyetelan angka, boleh langsung lewat sim |
| H11 | Kedalaman jangka panjang | Pemain mengatur prioritas perilaku AI per spirit | FFXII Gambit | Belum ditentukan | Jangka panjang, setelah rilis pertama |

Usulan UX yang tidak mengubah aturan (boleh langsung masuk prototipe): toggle auto-pass per spirit seperti stop di MTG Arena, layar chain yang menampilkan siapa yang masih bisa merespons dan sisa mana, garis target dan ikon status perilaku AI, dan ringkasan titik balik setelah match.

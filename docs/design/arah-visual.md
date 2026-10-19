# Arah Visual dan Presentasi Battle

Catatan: Upi memilih arena 2D dilihat dari atas seperti stadion (keputusan D9). Usulan kamera samping dan panggung wayang di dokumen ini tidak dipakai. Bagian narasi dan pipeline animasi berbasis rig masih bisa jadi referensi.

Status: kajian, menunggu keputusan Upi. Kamera 2D samping sudah condong disetujui. Yang dikaji di sini adalah gaya visualnya.

## Kenapa ini keputusan besar

Art adalah biaya terbesar untuk solo dev, sekaligus wajah game di halaman Steam. Gaya visual juga menentukan seberapa rumit AI posisi dan animasi. Karena itu gaya dipilih dengan tiga syarat: murah dibuat sendiri, mudah dibaca dalam battle 2v2, dan langsung terlihat beda di antara ribuan game lain.

## Opsi

| Opsi | Contoh | Biaya art | Keterbacaan | Kekhasan di Steam | Catatan |
| --- | --- | --- | --- | --- | --- |
| A. 2D samping, animasi skeletal biasa | Darkest Dungeon, battle Pokémon klasik | Sedang | Tinggi | Rendah sampai sedang | Aman, tapi mudah terlihat generik |
| B. Papan dan kartu: spirit sebagai ilustrasi yang bergoyang saat beraksi | Backpack Battles, The Bazaar, Super Auto Pets | Rendah | Tinggi | Rendah | Paling murah. Kurang terasa battle, dan intro/outro sulit terlihat megah |
| C. Panggung wayang: 2D samping, spirit sebagai boneka berengsel, dalang sebagai narator | Wayang kulit. Narasi reaktif ala Bastion, Darkest Dungeon, Hades | Rendah sampai sedang | Sangat tinggi, karena siluet | Sangat tinggi | Perlu sentuhan modern dan hormat budaya |
| D. 3D chibi dengan kamera tetap | Pokémon Champions, Temtem | Tinggi | Sedang | Rendah | Mahal, dan bersaing visual langsung dengan Pokémon Champions |
| E. Pixel art | Cassette Beasts, Coromon | Rendah sampai sedang | Tinggi | Rendah | Pasar creature game pixel art sudah padat |

## Rekomendasi: panggung wayang

Pemain berperan sebagai dalang yang memanggil spirit ke atas kelir (layar wayang). Spirit punya kehendak sendiri dan bertarung otomatis, tapi dalang yang menentukan momen penting: kapan jurus besar keluar, kapan menangkis, kapan mengganti boneka. Tema ini sekaligus memberi alasan cerita kenapa battle-nya auto battle dengan perintah, dan cocok dengan nama Pemanggil Spirit.

Alasannya:

1. Kamera samping adalah bentuk asli wayang. Boneka wayang memang dilihat dari samping.
2. Boneka wayang berengsel dan digerakkan lewat batang (tuding). Rig 2D cukup 5 sampai 8 bagian per spirit. Gerakan yang terbatas justru terasa autentik, bukan murahan. Ini prinsip yang sama dengan Paper Mario dan Darkest Dungeon: keterbatasan dijadikan gaya.
3. Siluet adalah bentuk visual yang paling mudah dibaca. Dengan dua spirit per sisi, pemain langsung tahu siapa di mana.
4. Kekhasan di Steam. Gaya ini langsung beda dari creature game lain, dan juga menjauhkan desain dari kesan Pokémon, yang mengurangi risiko IP.
5. Cerita kreator. Game dari Indonesia yang mengangkat identitas lokal pernah mendapat perhatian internasional, misalnya A Space for the Unbound (Mojiken) dan DreadOut (Digital Happiness).
6. Sudah ada bukti teknis. Projection: First Light (Shadowplay Studios, 2020) adalah game Steam yang terinspirasi wayang kulit. Genrenya puzzle-platformer, jadi bukan pesaing, tapi membuktikan gaya ini bisa jadi game yang utuh.

### Elemen game dan wujud wayangnya

| Elemen game | Wujud di panggung wayang |
| --- | --- |
| Arena | Kelir, diterangi blencong (lampu wayang) |
| Spirit | Boneka berengsel gaya wayang, makhluk orisinal. Wayang kancil (wayang dengan tokoh hewan) jadi preseden budaya untuk tokoh makhluk |
| Bench | Simpingan: boneka yang ditancapkan di batang pisang (gedebog) di sisi kiri dan kanan kelir |
| Pemain | Dalang |
| Narasi battle | Kalimat dalang dalam bentuk subtitle singkat, nanti bisa ditambah voice line |
| Pembukaan match | Gunungan ditancapkan di tengah kelir lalu dicabut |
| Resonansi | Gunungan diangkat di belakang spirit, lalu spirit berubah wujud |
| Fase Akhir | Gamelan naik tempo, nyala blencong mulai berkedip |
| KO | Boneka dicabut dan ditarik keluar dari kelir |

Dua lapisan visual sekaligus: boneka berwarna terlihat dari sisi dalang, dan bayangannya jatuh di kelir di belakangnya. Bayangan dibuat otomatis dari sprite yang sama lewat shader, tanpa gambar tambahan. Hasilnya kedalaman visual dengan biaya hampir nol.

### Risiko

1. Kesan tradisional atau edukatif. Penawarnya desain spirit yang berani, VFX modern, tempo cepat, dan musik gamelan yang dipadukan dengan unsur modern.
2. Hormat budaya. Wayang diakui UNESCO sebagai warisan budaya tak benda. Pakai tokoh orisinal, jangan menjadikan tokoh pewayangan yang sakral sebagai petarung, libatkan dalang atau perajin wayang sebagai konsultan, dan cantumkan kredit.
3. Pasar internasional belum tentu langsung paham. Karena itu style test di bawah wajib melibatkan pemain non-Indonesia.

## Narrative battle

Battle ditampilkan seperti pertunjukan dengan dalang yang bercerita. Ada tiga lapisan, dari yang paling murah:

1. Visual: boneka bergerak sendiri (autobattle). Ini yang paling sering terlihat.
2. Subtitle dalang di bawah layar, hanya untuk momen penting: pembukaan, chain besar, Lelah, KO, Resonansi, Fase Akhir. Maksimal satu baris setiap 8 detik, supaya tidak melelahkan.
3. Voice line dalang, dikerjakan setelah grey-box terbukti seru. Satu pengisi suara, sekitar 150 sampai 200 baris pendek.

Preseden: Bastion (narator yang bereaksi ke aksi pemain), Darkest Dungeon (narator mengomentari serangan kritis dan kematian), dan Hades (narator membacakan aksi tokoh utama). Di ketiga game itu, narasi jadi identitas dengan biaya jauh lebih kecil dari animasi tambahan.

Aturannya: narasi tidak boleh jadi satu-satunya sumber informasi. Semua kondisi battle tetap terlihat di HUD.

Contoh baris dalang:

- "Karang menancap di tengah kelir. Tak ada yang lewat."
- "Bara melesat ke belakang barisan!"
- "Tiga kali menangkis... Bara kehabisan napas."
- "Gunungan terangkat. Kabut tak lagi sama."

## Pipeline art untuk solo dev

1. Satu ilustrasi per spirit, dipotong 5 sampai 8 bagian: badan, kepala, dua lengan bertangkai, dan ekor atau aksesori.
2. Rig dengan paket 2D Animation bawaan Unity (gratis). Animasi meniru gerak tuding dalang.
3. Bayangan di kelir dibuat otomatis dari sprite yang sama (shader siluet dan blur).
4. Animasi unik per spirit: idle, serang, Skill Utama, intro, KO. Sisanya pose bersama dan VFX bersama.
5. Wujud Resonansi: variasi warna ditambah satu ornamen, bukan gambar baru.

Perkiraan untuk roster 20 spirit: 20 ilustrasi dan sekitar 100 animasi pendek berbasis rig, jauh lebih ringan dari animasi frame per frame.

## Langkah berikutnya

Buat style test sebelum memutuskan: 1 spirit, 1 kelir, 3 animasi (idle, Skill Utama, KO), bayangan, dan 1 VFX. Tunjukkan ke 5 sampai 10 orang, termasuk yang bukan orang Indonesia, lalu tanyakan dua hal: apakah langsung paham ini battle, dan apakah mereka tertarik. Style test bisa dikerjakan paralel dengan grey-box karena tidak menyentuh core sim.

# Log Keputusan

Aturan yang berlaku ada di [spec prototipe](spec-prototipe.md), yang disusun dari [v3.3](v3.3-status.md) dan keputusan di bawah. Perubahan aturan dicatat di sini dulu, lalu spec diperbarui. Aturan dikunci sampai grey-box dimainkan.

## Diputuskan

| ID | Tanggal | Keputusan | Menggantikan | Detail |
| --- | --- | --- | --- | --- |
| D1 | 2026-10-18 | Chain memakai pengurangan damage, bukan pembatalan. Satu spirit boleh menaruh lebih dari satu reaksi per chain selama mana dan beban cukup | v3.3 bagian Chain dan Burst, asumsi A4 dan A17, hipotesis H3, H4, H12 | [Chain dan tipe](chain-dan-tipe.md). Angka masih usulan |
| D2 | 2026-10-18 | Luka ringan: sebagian damage pulih saat spirit di bench | v3.3 "Bench tidak memulihkan HP", hipotesis H14 | [Diskusi 2026-10-18](diskusi-2026-10-18.md), poin 3 |
| D3 | 2026-10-18 | Menang saat lawan kehilangan 3 dari 4 spirit. Last Stand dihapus | v3.3 Last Stand dan kemenangan lewat 4 KO, asumsi A11, hipotesis H1, H2, H15 | [Diskusi 2026-10-18](diskusi-2026-10-18.md), poin 8. Asumsi A18 untuk KO bersamaan |
| D4 | 2026-10-18 | Tujuan dan empat pilar dikunci: pelatih bukan pemain, manajemen terbuka, aksi-reaksi adalah jantung, naluri yang bisa dipelajari | Draft pilar di dokumen status v3.3 | [Pilar](pillars.md) |
| D5 | 2026-10-18 | Loadout tetap tersembunyi di ranked dan terbuka di turnamen, sebagai sumber bluff | Pertanyaan apakah loadout dibuka | [Pilar](pillars.md), ketegangan nomor 1 |
| D6 | 2026-10-18 | Semua aturan disatukan ke spec prototipe sebagai acuan tunggal kode. Putaran playtest pertama sesempit mungkin: tipe sebagai efisiensi, luka ringan (D2), intro/outro, dan Resonansi dibangun sebagai toggle yang mati | Dokumen aturan yang terpecah, asumsi A0 | [Spec prototipe](spec-prototipe.md) |
| D7 | 2026-10-19 | Chain memakai reaksi spesial yang punya efek. Angka tetap dijumlah, efek diselesaikan dari reaksi teratas ke bawah. Semua yang dipasang di chain disebut reaction, bukan kartu | Chain yang isinya hanya angka (D1 tetap jadi tulang punggung) | [Diskusi 2026-10-19](diskusi-2026-10-19.md). Daftar reaksi menunggu persetujuan |
| D8 | 2026-10-19 | Setiap pihak hanya boleh memasang satu reaksi bernama sama per chain | Pertanyaan terbuka nomor 2 di spec | [Diskusi 2026-10-19](diskusi-2026-10-19.md) |
| D9 | 2026-10-19 | Arah visual: arena 2D dilihat dari atas seperti stadion, gaya pixel art | Usulan kamera samping dan panggung wayang | [Diskusi 2026-10-19](diskusi-2026-10-19.md), bagian arena top-down |
| D10 | 2026-10-19 | Hipotesis H17 dihapus. Basic Reaction hanya boleh dipakai kalau cocok attack type. Toggle `typeMatchMode` dihapus | H17, toggle mode efisiensi | [Diskusi 2026-10-19](diskusi-2026-10-19.md), bagian lubang tipe |
| D11 | 2026-10-19 | Istilah game memakai bahasa Inggris, di dokumen maupun di game. Prosa dokumen tetap bahasa Indonesia | Istilah berbahasa Indonesia (Skill Utama, Lelah, beban, dan lainnya) | Tabel istilah di CLAUDE.md |
| D12 | 2026-10-19 | Daftar 18 reaction disetujui sebagai satu set: Dodge, Shield, Interrupt, Press, Intercept, Retreat, Lock, Counter, Reflect, Cancel, Steal, Pressure, Dominate, Pierce, Execute, Endure, Heal, Burst | Usulan set awal dan lanjutan, hipotesis H18 | [Spec prototipe](spec-prototipe.md) bagian 8 |
| D13 | 2026-10-19 | Pool 3 reaction per spirit (1 Basic dan 2 Special), membawa 2. Press gratis untuk semua spirit, Burst khusus Controller | Pool 3 tanpa struktur | [Spec prototipe](spec-prototipe.md) bagian 3 |
| D14 | 2026-10-19 | Sistem jarak: pita Self, Close, Mid, Global. Interrupt sejauh jangkauan Cast. Jangkauan Main Skill per attack type. Jarak diukur saat chain dibuka | Reaction tanpa syarat posisi, hipotesis H19 | [Spec prototipe](spec-prototipe.md) bagian 4 |
| D15 | 2026-10-19 | Formation tim (Compact, Balanced, Aggressive) sebagai instruksi pelatih, terlihat lawan, jeda 30 tick | Posisi sepenuhnya ditentukan AI, hipotesis H20 | [Spec prototipe](spec-prototipe.md) bagian 4 |
| D16 | 2026-10-19 | Tujuh perbaikan audit P0: jam dunia beku selama chain, satu chain pada satu waktu dengan urutan Speed lalu slot, chain terbuka dari informasi publik dan pass tampil minimal 1 detik, waktu dasar 4 detik per giliran ditambah bank 30 detik, target 5 sampai 7 menit jam dunia dan di bawah 10 menit nyata, Vulnerable diganti Exposed, penalti -30% ke target stun dihapus | Dunia 10% selama chain, bank 35 detik, Vulnerable 2 detik, penalti stun | [Audit 2026-10-19](audit-2026-10-19.md) P0 |
| D17 | 2026-10-19 | PvP async adalah tantangan tim dengan ghost yang memakai instruksi dari pemiliknya. Ranked yang sesungguhnya adalah PvP live | Ghost dengan preset tanpa definisi | [Audit 2026-10-19](audit-2026-10-19.md) P1 nomor 8 |
| D18 | 2026-10-19 | Tipe Resonance adalah bagian dari loadout, jadi tersembunyi di ranked dan terbuka di turnamen | Tipe Resonance di luar definisi loadout | [Audit 2026-10-19](audit-2026-10-19.md) P2 nomor 14 |

## Menunggu keputusan Upi

| Topik | Usulan | Detail |
| --- | --- | --- |
| Resonance | Terbuka lewat syarat role yang terlihat kedua pemain | Hipotesis H16 |
| Format | Tetap 2v2 sesuai batasan. Tidak ada perubahan kecuali Upi meminta | [Diskusi 2026-10-18 bagian 2](diskusi-2026-10-18-2.md) |

## Ditunda

Upi memilih fokus ke combat dulu. Topik berikut dibahas lagi setelah combat inti terbukti seru.

| Topik | Usulan | Detail |
| --- | --- | --- |
| Mode PvE | Mode puzzle chain sebagai isi single-player dan tutorial, ditambah gauntlet melawan tim AI | Hipotesis H21, [audit 2026-10-19](audit-2026-10-19.md) P1 nomor 9 |
| Akses roster | Roster dasar terbuka untuk ranked sejak awal, progresi hanya kosmetik atau PvE | Hipotesis H22, [audit 2026-10-19](audit-2026-10-19.md) P1 nomor 10 |

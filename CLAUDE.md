# CLAUDE.md

Panduan untuk Claude Code di repo ini. Baca seluruhnya sebelum mengerjakan apa pun.

## Proyek

Pemanggil Spirit (judul kerja) adalah game PvP battle 2v2 untuk PC/Steam, premium sekali beli, dibuat solo oleh Upi dengan Unity. Spirit bertarung otomatis sesuai role. Pemain mengatur kapan Main Skill dipakai, kapan menjawab serangan lewat chain aksi-reaksi, Formation tim, dan kapan merotasi spirit. Sumber dayanya: mana bersama, timeline, load, dan posisi di arena.

Fase sekarang: membangun prototipe grey-box dari `docs/design/spec-prototipe.md`. Arah visual: arena 2D dilihat dari atas seperti stadion, gaya pixel art (keputusan D9). Belum ada kode di repo. Sim C# lama (`pemanggil-sim-m1.zip`, aturan v4, 82 tes) ada di tangan Upi dan akan di-port ke sini.

## Pilar (dikunci)

Tujuan: pemain adalah pelatih. Spirit bertarung dengan nalurinya sendiri, dan pemain menang lewat strategi, timing, dan cara memakai sumber daya, dengan rasa puas seperti menang poker. Urutan pilar menentukan siapa yang menang kalau dua pilar bertentangan:

1. Pelatih, bukan pemain. Setiap hasil bisa ditelusuri ke keputusan pemain, bukan kecepatan tangan atau keberuntungan.
2. Manajemen terbuka. Semua kondisi battle terlihat, kecuali loadout di ranked sebagai ruang bluff. Menang karena efisien, kalah karena serakah.
3. Aksi-reaksi adalah jantung. Chain adalah ronde taruhan sumber daya, dan setiap chain harus berisi keputusan nyata.
4. Naluri yang bisa dipelajari. Setiap spirit punya naluri yang konsisten dan bisa diantisipasi.

Bukan game ini: tebak-tebakan, adu kecepatan tangan, game auto yang cukup ditonton. Detail ada di `docs/design/pillars.md`.

## Dokumen

Yang berlaku:

1. `docs/design/spec-prototipe.md`: satu-satunya acuan aturan untuk kode. Wajib dibaca sebelum menyentuh logika battle.
2. `docs/design/pillars.md`: tujuan dan empat pilar. Dasar menilai setiap usulan.
3. `docs/design/keputusan.md`: log keputusan Upi. Setiap perubahan aturan dicatat di sini dulu, lalu spec diperbarui.
4. `docs/design/hypothesis-backlog.md`: usulan yang menunggu playtest.

Latar belakang, tidak diperbarui lagi dan bukan acuan aturan: `v3.3-status.md`, `chain-dan-tipe.md`, `prototype-assumptions.md`, `review-v3.3.md`, `gambaran-battle.md`, `arah-visual.md`, `audit-*.md`, dan `diskusi-*.md` di `docs/design/`.

## Aturan desain (paling penting)

- Aturan yang berlaku hanya `docs/design/spec-prototipe.md`, dan beku sampai prototipe dimainkan. Perubahan hanya kalau Upi memutuskan secara eksplisit: catat di `keputusan.md`, lalu perbarui spec. Jangan membuat v3.4, v4, atau versi baru di atas kertas, walaupun diminta "menyempurnakan". Ide baru masuk ke backlog hipotesis dengan format: masalah, perubahan, preseden, metrik lolos.
- Hipotesis boleh diimplementasikan sebagai toggle di konfigurasi, mati secara default. Default hanya diubah oleh Upi, satu perubahan per putaran playtest.
- Kalau aturan ambigu saat coding, jangan menebak diam-diam. Tambahkan ke bagian Pertanyaan terbuka di spec dengan default yang dipakai, lalu sebutkan di jawaban.
- Jangan menghidupkan lagi mekanik yang diarsipkan (stance tersembunyi, commit reaction tersembunyi, feint, dorong/tarik icon lawan) kecuali Upi minta secara eksplisit. Semuanya tebak-tebakan atau sudah terbukti merusak. Loadout tersembunyi di ranked adalah bluff yang disengaja (D5), bukan pelanggaran.
- Setiap usulan aturan dinilai dengan satu pertanyaan: mendekatkan ke pilar, atau menjauhkan?
- Bukti datang dari playtest manusia. Hasil sim adalah petunjuk arah, bukan bukti.

## Terminologi

Selalu pakai "spirit". Jangan pernah menulis "pokemon" atau "Pokémon" di kode, nama aset, dokumen publik, atau commit. Alasannya legal: Nintendo dan The Pokémon Company aktif menggugat (kasus Palworld, 2024).

Istilah game memakai bahasa Inggris (keputusan D11), di dokumen maupun di game. Prosa dokumen tetap bahasa Indonesia. Dokumen latar belakang masih memakai nama lama berbahasa Indonesia, jadi kolom terakhir membantu membacanya.

Semua yang dipasang di chain disebut reaction, bukan kartu. Game ini bukan card game.

| Istilah | Kode | Nama lama |
| --- | --- | --- |
| Main Skill | `MainSkill` | Skill Utama |
| Auto Skill | `AutoSkill` | Skill Auto |
| Timeline, Ready, midline | `Timeline`, `Ready`, `Midline` | timeline icon, garis tengah |
| Charge | `Charge` | |
| Contact, Projectile, Cast | `AttackType.Contact`, `AttackType.Projectile`, `AttackType.Cast` | Kontak, Lontaran, Rapalan |
| Reaction, Basic Reaction, Special Reaction | `Reaction` | reaksi, tangkisan |
| Dodge, Shield, Interrupt | `ReactionType.Dodge`, `.Shield`, `.Interrupt` | Elak, Perisai, Sela |
| Press, Burst | `ReactionType.Press`, `.Burst` | Tekan |
| Intercept, Retreat, Lock | `ReactionType.Intercept`, `.Retreat`, `.Lock` | Hadang, Mundur, Kunci |
| Counter, Reflect, Cancel, Steal | `ReactionType.Counter`, `.Reflect`, `.Cancel`, `.Steal` | Balas, Pantul, Potong, Curi |
| Pressure, Dominate, Pierce, Execute | `ReactionType.Pressure`, `.Dominate`, `.Pierce`, `.Execute` | Desak, Tembus, Habisi |
| Endure, Heal | `ReactionType.Endure`, `.Heal` | Teguh, Pulih |
| Chain, layer, pass | `Chain`, `ChainLayer`, `Pass` | |
| Power, Block | `Power`, `Block` | Tahan |
| Full Block, Chip Damage, Type Match | `FullBlock`, `ChipDamage`, `TypeMatch` | tertahan penuh, tangkisan cocok tipe |
| Load, Fatigue | `ReactionLoad`, `Fatigue` | beban reaction, Lelah |
| Exposed | `Exposed` | Vulnerable |
| Perfect Swap, Emergency Swap | `PerfectSwap`, `EmergencySwap` | Swap Darurat |
| Intro, Outro | `Intro`, `Outro` | |
| Resonance | `Resonance` | Resonansi |
| Recoverable Damage | `RecoverableDamage` | luka ringan |
| Overtime | `Overtime` | Fase Akhir |
| Instinct | `Instinct` | naluri |
| Slow Zone | `SlowZone` | zona slow, zona lambat |
| Formation: Compact, Balanced, Aggressive | `Formation.Compact`, dan seterusnya | |
| Range: Self, Close, Mid, Global | `RangeBand.Self`, dan seterusnya | |
| Vanguard, Striker, Diver, Controller, Support | `Role.Vanguard`, dan seterusnya | |

## Arsitektur

Prinsip utama: aturan battle hidup di core sim C# murni yang deterministik, terpisah total dari Unity. Satu core yang sama dipakai untuk unit test, matriks bot untuk balance, lawan AI, ghost async, replay, dan nanti PvP live lockstep. Container cloud tempat Claude Code jalan tidak punya Unity, jadi core harus bisa dibangun dan dites dengan `dotnet` saja.

Arah dependensi: `Presentation -> AI -> Core`. Core tidak tahu apa pun tentang lapisan di atasnya.

Aturan Core (`Spirit.Core`):

- Tanpa `UnityEngine`. Asmdef memakai `noEngineReferences: true`.
- Deterministik: input log + seed + konfigurasi yang sama selalu menghasilkan state akhir yang sama. Ada tes hash untuk ini.
- Waktu dalam tick integer (1 tick = 100 ms). Tidak ada `float` atau `double` di logika. Persentase dalam basis point (10000 = 100%). Mana dalam milli-mana (regen 1 per 2 detik = 50 per tick).
- Tidak memakai `System.Random`, `DateTime`, `Guid`, thread, atau `Task`. RNG (kalau perlu) adalah PRNG sendiri dengan seed per match.
- Logika tidak bergantung pada urutan enumerasi `Dictionary` atau `HashSet`. Pakai array atau `List` dengan tie-break eksplisit (indeks slot spirit).
- API: `Match.Step(commands)` maju satu tick dan menghasilkan event. Command: `UseMainSkill`, `Charge`, `PerfectSwap`, `EmergencySwap`, `SetFormation`, `AddLayer`, `Pass`, `Resonate`, `Surrender`. Presentation membaca snapshot state dan event, tidak pernah mengubah state.
- Dua jam: jam dunia beku total selama chain, jam chain tetap berjalan dalam tick waktu nyata untuk batas waktu keputusan (D16).
- Posisi di arena memakai koordinat integer (1 tile = 100 unit). Gerak memakai steering sederhana tanpa navmesh.
- Semua angka aturan ada di konfigurasi (data), tidak di-hard-code. Core menerima objek konfigurasi yang sudah jadi. Parsing JSON dilakukan di luar Core.
- Resolver chain di balik interface (`IChainResolver`). Default-nya model pengurangan damage (keputusan D1). Model pembatalan v3.3 disimpan sebagai pembanding di sim.

Batas bahasa C#: Unity 6 memakai C# 9 dan .NET Standard 2.1, jadi Core dan AI tidak boleh memakai file-scoped namespace, global using, `required`, raw string literal, atau list pattern. `record` dan `init` butuh shim `IsExternalInit`. Hindari kecuali shim sudah ada.

AI (`Spirit.AI`): utility AI. Setiap primitif perilaku (Engage, Dive, Peel, Disengage, Zone, pemilihan target) diberi skor, lalu dikalikan bobot role. Satu sistem, lima preset role. Bot sim dan ghost async memakai antarmuka keputusan yang sama dengan pemain manusia. Tie-break harus deterministik karena AI yang tidak bisa ditebak terasa tidak adil di PvP.

Presentation (`Spirit.Presentation`): MonoBehaviour, view, input, UI, efek. Tidak ada logika aturan di sini. Kalau butuh logika, logika itu milik Core.

## Struktur folder (target, belum dibuat)

```
CLAUDE.md
docs/design/                  dokumen desain (Bahasa Indonesia)
Game/                         project Unity
  Assets/_Project/
    Core/                     Spirit.Core.asmdef, aturan battle
    Core.Tests/               Spirit.Core.Tests.asmdef, EditMode, NUnit
    AI/                       Spirit.AI.asmdef, bot dan ghost, C# murni
    Presentation/             Spirit.Presentation.asmdef
    Data/                     konfigurasi aturan dan definisi spirit (JSON)
    Scenes/
tools/sim/                    proyek .NET yang me-link source Core dan AI
  Spirit.Sim/                 console: matriks bot, log CSV
  Spirit.Sim.Tests/           menjalankan tes yang sama dengan Core.Tests
```

`tools/sim` me-link file `.cs` dari `Game/Assets/_Project/Core` dan `AI` (via `<Compile Include=...>`), bukan menyalin. Satu sumber, dua target build.

## Build dan tes

Belum ada perintah yang jalan karena kode belum ada. Setelah sim di-port, targetnya:

```bash
dotnet test tools/sim/Spirit.Sim.Tests          # semua tes Core dan AI
dotnet run --project tools/sim/Spirit.Sim -- --matrix --seeds 200   # matriks bot
```

Kalau `dotnet` belum terpasang di container, pasang .NET SDK 8. Di Unity, tes yang sama jalan lewat Test Runner (EditMode).

Setiap aturan punya tes unit yang namanya menyebut aturannya, misalnya `Chain_DefenderTakesFirstTurn`. Perbaikan bug diawali tes yang gagal. Jangan pernah melewati, menonaktifkan, atau menghapus tes supaya hijau.

## Metrik wajib di sim

Sim harus mencatat metrik yang menjawab pertanyaan playtest dan masalah yang diketahui:

- Distribusi kedalaman chain, mana yang dibakar tiap pihak, dan damage akhir per chain.
- Untuk setiap Main Skill yang mendarat: alasan defender tidak menjawab (ikon belum lewat midline secara alami, ikon mundur karena reaction sebelumnya, mana kurang, Fatigue, stun, tipe tidak cocok, di luar jarak, memilih pass).
- Porsi damage otomatis dan porsi damage intro dari total damage.
- Jeda terpanjang tanpa aksi per pemain.
- Jumlah dan total durasi slow-mo per menit.
- Keputusan per menit per pemain.
- Prediktabilitas chain: seberapa sering hasil chain bisa ditebak dari state yang terlihat saat chain terbuka.
- Jumlah chain per menit, rata-rata durasi slow-mo per chain, dan durasi match dalam jam dunia maupun waktu nyata.
- Seberapa sering tiap reaction dibawa dan dipakai, kontribusinya ke kemenangan, dan seberapa sering tiap role (terutama Controller) dipilih.
- Pemakaian tiap Formation.

## Batas scope solo dev

- Jangan membangun netcode, backend, akun, monetisasi, atau pipeline art sebelum grey-box terbukti seru.
- Urutan rilis: lawan AI, lalu PvP async (tantangan tim dengan ghost yang memakai instruksi pemiliknya), lalu PvP live sebagai ranked yang sesungguhnya (D17).
- Scope putaran pertama mengikuti bagian 1 di `spec-prototipe.md`.
- Pilih solusi paling sederhana yang bisa dimainkan. Abstraksi baru hanya kalau sudah dibutuhkan dua kali.

## Unity dan git

- Unity 6 LTS (pakai rilis LTS terbaru saat project dibuat), URP, Input System.
- Serialization Force Text dan Visible Meta Files (default Unity 6). File `.meta` selalu ikut di-commit.
- Git LFS dipasang sebelum aset biner pertama (gambar, audio, model) masuk repo.

## Cara bekerja dengan Upi

- Jawab dalam Bahasa Indonesia yang natural dan santai tapi jelas. Istilah teknis bahasa Inggris boleh dipakai kalau memang lazim.
- Dokumen desain ditulis dalam Bahasa Indonesia. Kode, identifier, komentar kode, dan pesan commit dalam bahasa Inggris.
- Ringkas dan langsung ke inti. Jangan pakai em dash di dokumen maupun jawaban.
- Berperan sebagai senior game dev yang skeptis: uji asumsi di balik permintaan, beri data dari game yang sudah ada, dan bantah kalau arahnya merugikan. Tapi tetap hormati pembekuan spec.
- Commit kecil dan sering. Satu commit, satu perubahan yang utuh.

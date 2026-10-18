# CLAUDE.md

Panduan untuk Claude Code di repo ini. Baca seluruhnya sebelum mengerjakan apa pun.

## Proyek

Pemanggil Spirit (judul kerja) adalah game PvP battle 2v2 untuk PC/Steam, premium sekali beli, dibuat solo oleh Upi dengan Unity. Spirit bertarung otomatis sesuai role. Pemain mengatur kapan Skill Utama dipakai, kapan menjawab serangan lewat chain aksi-reaksi, dan kapan merotasi spirit. Sumber dayanya: mana bersama, timeline icon, dan beban reaction.

Fase sekarang: membangun prototipe grey-box dari aturan v3.3. Belum ada kode di repo. Sim C# lama (`pemanggil-sim-m1.zip`, aturan v4, 82 tes) ada di tangan Upi dan akan di-port ke sini.

## Dokumen sumber kebenaran

Baca sesuai kebutuhan, dengan urutan prioritas ini:

1. `docs/design/v3.3-status.md`: aturan battle yang berlaku. Beku.
2. `docs/design/prototype-assumptions.md`: jawaban untuk hal yang belum didefinisikan dokumen status. Wajib dibaca sebelum menyentuh logika battle.
3. `docs/design/pillars.md`: pilar dan batasan. Masih draft.
4. `docs/design/hypothesis-backlog.md`: usulan perubahan yang menunggu playtest.
5. `docs/design/review-v3.3.md`: analisis, data pasar, dan alasan di balik backlog.
6. `docs/design/gambaran-battle.md`: gambaran visual dan alur battle dengan bahasa sederhana. Acuan untuk presentation dan UI, bukan aturan.

## Aturan desain (paling penting)

- v3.3 beku sampai prototipe dimainkan. Jangan membuat v3.4, v4, atau versi baru di atas kertas, walaupun diminta "menyempurnakan". Ide baru masuk ke backlog hipotesis dengan format: masalah, perubahan, preseden, metrik lolos.
- Hipotesis boleh diimplementasikan sebagai toggle di konfigurasi, mati secara default. Default hanya diubah oleh Upi, satu perubahan per putaran playtest.
- Kalau aturan ambigu saat coding, jangan menebak diam-diam. Pakai default di `prototype-assumptions.md`. Kalau belum ada, tambahkan baris baru dengan default dan status "menunggu Upi", lalu sebutkan di jawaban.
- Jangan menghidupkan lagi mekanik yang diarsipkan (stance tersembunyi, commit reaction tersembunyi, feint, dorong/tarik icon lawan) kecuali Upi minta secara eksplisit. Semuanya bertentangan dengan pilar manajemen atau sudah terbukti merusak.
- Setiap usulan aturan dinilai dengan satu pertanyaan: mendekatkan ke pilar, atau menjauhkan?
- Bukti datang dari playtest manusia. Hasil sim adalah petunjuk arah, bukan bukti.

## Terminologi

Selalu pakai "spirit". Jangan pernah menulis "pokemon" atau "Pokémon" di kode, nama aset, dokumen publik, atau commit. Alasannya legal: Nintendo dan The Pokémon Company aktif menggugat (kasus Palworld, 2024).

Istilah desain berbahasa Indonesia, identifier kode berbahasa Inggris:

| Desain | Kode |
| --- | --- |
| Skill Utama | `MainSkill` |
| Skill Auto | `AutoSkill` |
| Timeline icon, Ready | `Timeline`, `Ready` |
| Charge | `Charge` |
| Kontak, Lontaran, Rapalan | `AttackType.Contact`, `AttackType.Projectile`, `AttackType.Cast` |
| Dodge, Shield, Interrupt, Burst | `ReactionType.Dodge`, `.Shield`, `.Interrupt`, `.Burst` |
| Chain, layer, pass | `Chain`, `ChainLayer`, `Pass` |
| Beban reaction, Lelah | `ReactionLoad`, `Fatigue` |
| Vulnerable | `Vulnerable` |
| Perfect Swap, Swap Darurat | `PerfectSwap`, `EmergencySwap` |
| Intro, Outro | `Intro`, `Outro` |
| Resonansi | `Resonance` |
| Last Stand | `LastStand` |
| Fase Akhir | `Overtime` |
| Vanguard, Striker, Diver, Controller, Support | `Role.Vanguard`, dan seterusnya |

## Arsitektur

Prinsip utama: aturan battle hidup di core sim C# murni yang deterministik, terpisah total dari Unity. Satu core yang sama dipakai untuk unit test, matriks bot untuk balance, lawan AI, ghost async, replay, dan nanti PvP live lockstep. Container cloud tempat Claude Code jalan tidak punya Unity, jadi core harus bisa dibangun dan dites dengan `dotnet` saja.

Arah dependensi: `Presentation -> AI -> Core`. Core tidak tahu apa pun tentang lapisan di atasnya.

Aturan Core (`Spirit.Core`):

- Tanpa `UnityEngine`. Asmdef memakai `noEngineReferences: true`.
- Deterministik: input log + seed + konfigurasi yang sama selalu menghasilkan state akhir yang sama. Ada tes hash untuk ini.
- Waktu dalam tick integer (1 tick = 100 ms). Tidak ada `float` atau `double` di logika. Persentase dalam basis point (10000 = 100%). Mana dalam milli-mana (regen 1 per 2 detik = 50 per tick).
- Tidak memakai `System.Random`, `DateTime`, `Guid`, thread, atau `Task`. RNG (kalau perlu) adalah PRNG sendiri dengan seed per match.
- Logika tidak bergantung pada urutan enumerasi `Dictionary` atau `HashSet`. Pakai array atau `List` dengan tie-break eksplisit (indeks slot spirit).
- API: `Match.Step(commands)` maju satu tick dan menghasilkan event. Command: `UseMainSkill`, `Charge`, `PerfectSwap`, `EmergencySwap`, `AddLayer`, `Pass`, `Resonate`, `Surrender`. Presentation membaca snapshot state dan event, tidak pernah mengubah state.
- Semua angka aturan ada di konfigurasi (data), tidak di-hard-code. Core menerima objek konfigurasi yang sudah jadi. Parsing JSON dilakukan di luar Core.
- Resolver chain di balik interface (`IChainResolver`) supaya hipotesis H3 dan H4 bisa diuji tanpa membongkar Core.

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

- Distribusi kedalaman chain dan pemenangnya per kedalaman.
- Untuk setiap Skill Utama yang mendarat: alasan pihak bertahan tidak menjawab (icon belum lewat garis tengah secara alami, icon mundur karena reaksi sebelumnya, mana kurang, Lelah, stun, tipe tidak cocok, memilih pass).
- Porsi damage otomatis dan porsi damage intro dari total damage.
- Jeda terpanjang tanpa aksi per pemain.
- Jumlah dan total durasi slow-mo per menit.
- Keputusan per menit per pemain.
- Durasi duel 1v1 dan durasi match.

## Batas scope solo dev

- Jangan membangun netcode, backend, akun, monetisasi, atau pipeline art sebelum grey-box terbukti seru.
- Urutan rilis: lawan AI, lalu PvP async (ghost), lalu PvP live.
- Scope grey-box mengikuti asumsi A0 di `prototype-assumptions.md`.
- Pilih solusi paling sederhana yang bisa dimainkan. Abstraksi baru hanya kalau sudah dibutuhkan dua kali.

## Unity dan git

- Unity 6 LTS (pakai rilis LTS terbaru saat project dibuat), URP, Input System.
- Serialization Force Text dan Visible Meta Files (default Unity 6). File `.meta` selalu ikut di-commit.
- Git LFS dipasang sebelum aset biner pertama (gambar, audio, model) masuk repo.

## Cara bekerja dengan Upi

- Jawab dalam Bahasa Indonesia yang natural dan santai tapi jelas. Istilah teknis bahasa Inggris boleh dipakai kalau memang lazim.
- Dokumen desain ditulis dalam Bahasa Indonesia. Kode, identifier, komentar kode, dan pesan commit dalam bahasa Inggris.
- Ringkas dan langsung ke inti. Jangan pakai em dash di dokumen maupun jawaban.
- Berperan sebagai senior game dev yang skeptis: uji asumsi di balik permintaan, beri data dari game yang sudah ada, dan bantah kalau arahnya merugikan. Tapi tetap hormati pembekuan v3.3.
- Commit kecil dan sering. Satu commit, satu perubahan yang utuh.

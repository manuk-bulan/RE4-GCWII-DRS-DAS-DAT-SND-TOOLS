# RE4 GC/WII DRS DAS DAT SND Tools

**RE4 GC/WII SND Audio Studio** by Manuk Bulan, version **v0.0.1**

A Windows tool for **Resident Evil 4 GameCube / Wii**. It unpacks and repacks the game's containers (`.drs`, `.das`, `.dat`, `.udas`, `.map`). It also lets you listen to, replace, export and import the sounds inside the sound banks (`_END.SND`, DSP-ADPCM).

The interface is available in **Bahasa Indonesia, English and Português**, and you can switch language at any time without restarting.

> 🇮🇩 Penjelasan Bahasa Indonesia ada di bagian [bawah](#-bahasa-indonesia).

<!-- Tambahkan screenshot di sini, misalnya:
![Main window](docs/screenshot_main.png)
![DRS Manager](docs/screenshot_drs.png)
-->

---

## Download

1. Download `re4_gcwii_drs_das_dat_snd_tools_v0.0.1.exe` from the **[Releases](https://github.com/manuk-bulan/RE4-GCWII-DRS-DAS-DAT-SND-TOOLS/releases)** page.
2. Run it. No installation is needed.

You can also drag a `.SND` or `.drs` file onto the exe to open it directly.

**Requirements:** Windows 10 / 11 with .NET Framework 4.5 or newer, which is already built into Windows 10/11.

---

## Features

### SND Audio Studio (sound bank editor)
- Opens RE4 GC/Wii sound banks: `.SND` (for example `pl00_END.SND`), or a `.drs` / `.das` directly.
- Sample list styled like Explorer, with these columns: index, name, sample rate, duration, loop, format/status and WAV size.
- **Play** any sample right away (no export needed).
- **Replace** samples with `.wav` (PCM 8/16/24/32-bit or float, mono or stereo) or with Nintendo `.dsp` (DSP-ADPCM, used as-is without re-encoding).
  - High-quality DSP-ADPCM encoding, several samples at once.
  - By default the WAV is resampled to the original sample rate automatically, so the sound bank does not grow too large.
  - The original loop is adjusted automatically when the sample rate changes.
- **Drag & drop**: drop a `.wav` / `.dsp` onto a row to replace that sample. Dropping many files at once matches them by name (`pl00_0003.wav` or `0003.wav`).
- **Export** the selected samples or all of them to `.wav` (plus optional `.dsp`).
- **Import folder**: only files that actually differ from the current samples are used. `.wav` takes priority, then `.dsp`.
- **Edit loop**: change the loop start and end, or turn the loop off, without re-encoding the sound.
- **Multi mode**: open two sound banks side by side and copy samples between them. ADPCM-to-ADPCM copies are exact, with no quality loss.
- **Safe saving**:
  - the game's ARAM limit is checked before writing (player `0x130000`, weapon/core `0x40000`, other banks may not grow);
  - a `.bak` backup is made once;
  - the original file stays intact if saving fails.
- Opening and saving a bank without any change gives a file identical to the original.

### DRS / DAS Manager (container unpack / repack)
- Extracts `.drs`, `.das`, `.dat`, `.udas`, `.map` and `.decmp`, Final and E3 versions.
  - Output: a `<name>\` folder plus a `<name>.idxbig` manifest.
- Also reads `.idxbig` manifests in the older `KEY:VALUE` format.
- Shows every part with its offset, size and status:
  - **Modified**: you edited the file after extracting;
  - **MISSING**: the file is gone;
  - **Replaced from .DMY**.
- Double-click `_END.SND` to open it in the Studio. Before repacking, the tool makes sure your sound changes have been saved.
- **Repack** to the original file (with a backup) or **Repack As** to a new file.

---

## Usage

### DRS → SND → DRS workflow
1. Click **Open DRS** (`Ctrl+D`) and choose `pl00.drs`, or drag the file into the window.
   - The DRS Manager extracts it to `pl00\` plus `pl00.idxbig`.
   - If it was already extracted, you can choose to **reuse** the existing extraction (safe, your edits are kept) or **extract again** (which overwrites the folder).
2. Double-click `pl00_END.SND` (or click **Open SND in Studio**).
3. Edit the sounds in the Studio, then click **Save**.
4. Go back to the DRS Manager and click **Repack** (`Ctrl+S`), or **Repack As...** to write a new file.

Other extracted files (`.BIN`, `.TPL`, ...) can be replaced as well. Their status shows up in the DRS Manager.

### Editing a sound bank
| Action | How |
|---|---|
| Play / stop | `Space`, `Enter`, double-click / `Esc` |
| Replace with WAV/DSP | **Replace WAV** (`Ctrl+R`) or drag a file onto the row |
| Export selected | **Export ▾ → Export selected WAV / DSP** (`Ctrl+E`) |
| Export all | **Export ▾ → Export all samples to a folder** (`Ctrl+Shift+E`) → `<name>_unpack\` |
| Import edited folder | **Export ▾ → Import folder** (`Ctrl+I`) |
| Edit loop | `Ctrl+L` |
| Revert to original | `Ctrl+Z` (selected) or **Revert all changes** |
| Save / Save As | `Ctrl+S` / `Ctrl+Shift+S` |

Tip: export all samples, edit the `.wav` files you want in your audio editor, then use **Import folder**. Only the files you changed are used.

### Multi mode
Click **Mode: Single / Multi** (`Ctrl+M`) and open a sound bank on each side. Select the source on one side and the target on the other, then click **Move Left** / **Move Right** (`Ctrl+←` / `Ctrl+→`).
- When the same number of samples is selected on both sides, they are paired in order.
- One source can be copied to many targets.
- If nothing is selected on the target side, the sample is copied to the same index number.

### Keyboard shortcuts
| Studio | DRS Manager |
|---|---|
| `Ctrl+O` open, `Ctrl+D` DRS Manager | `Ctrl+O` open |
| `Ctrl+S` save, `Ctrl+Shift+S` save as | `Ctrl+S` repack, `Ctrl+Shift+S` repack as |
| `Ctrl+R` replace, `Ctrl+L` loop, `Ctrl+Z` revert | `Enter` open row, `F5` refresh |
| `Ctrl+E` / `Ctrl+Shift+E` export, `Ctrl+I` import | |
| `Ctrl+M` mode, `Tab` switch side, `Ctrl+A` select all | |

### Settings (⚙)
- Language: Indonesia / English / Português.
- Match the WAV sample rate to the original sample (default **on**).
- Adjust the original loop when the sample rate changes (default **on**).
- `.bak` backup before Save / Repack overwrites a file (default **on**).
- Also export a `.dsp` next to each `.wav` (default **on**).
- Auto-play when a sample is selected.
- **Ignore the ARAM limit (RISKY)**: going over the limit can corrupt sounds or crash the game.

Settings are stored in `re4_gcwii_drs_das_dat_snd_tools.ini` next to the exe.

---

## Notes & limits
- The number of samples in a bank is fixed: you can replace samples but not add new ones.
- Usable sample rates are 1000 to 65535 Hz (the game's limit).
- If a sound is too long or its sample rate too high, the bank may exceed the ARAM limit and saving is refused. Shorten the sound or lower its sample rate.
- A red warning in the status column means the sample shares its ADPCM settings with another sample. Check both in game.
- Always keep a backup of your original game files.

---

## Credits
- **Manuk Bulan**: tool and GUI.

## Disclaimer
This is a fan-made modding tool. It is not affiliated with or endorsed by Capcom or Nintendo. *Resident Evil 4* is a trademark of Capcom.

---

## 🇮🇩 Bahasa Indonesia

**RE4 GC/WII SND Audio Studio** adalah tool Windows untuk modding suara dan container **Resident Evil 4 GameCube / Wii**.

**Download:** ambil `re4_gcwii_drs_das_dat_snd_tools_v0.0.1.exe` dari halaman **Releases**, lalu jalankan. Tidak perlu install, cukup Windows 10/11.

**Fitur utama**
- **DRS / DAS Manager**:
  - extract dan repack `.drs`, `.das`, `.dat`, `.udas`, `.map`, `.decmp`;
  - menandai file yang diubah atau hilang;
  - bisa membaca `.idxbig` format lama (`KEY:VALUE`).
- **SND Audio Studio**:
  - putar, ganti (WAV / DSP), export, import, dan edit loop sampel di `_END.SND`;
  - drag & drop `.wav` / `.dsp` langsung ke baris sampel;
  - Mode Multi untuk menyalin sampel antar dua bank tanpa turun kualitas;
  - batas ARAM game dicek sebelum menyimpan, dan backup `.bak` dibuat otomatis.
- Bahasa Indonesia / English / Português, bisa diganti langsung dari Pengaturan (⚙).

**Alur kerja**
1. **Buka DRS** (`Ctrl+D`) → pilih `pl00.drs` → otomatis di-extract ke folder `pl00\`.
2. Klik 2x `pl00_END.SND` → edit suaranya di Studio → **Simpan**.
3. Kembali ke DRS Manager → **Repack**.

**Tips:** Export semua sampel, edit `.wav` yang diinginkan, lalu **Import folder**. Hanya file yang diubah yang dipakai.

**Catatan**
- Jumlah sampel tidak bisa ditambah, hanya diganti.
- Kalau suara terlalu panjang atau sample rate terlalu tinggi, penyimpanan ditolak karena melewati batas ARAM.
- Selalu simpan backup file game asli.

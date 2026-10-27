# Poda-poda — Optical Card Organ ("Sun Organ")

Instrumen elektronik DIY: **kartu kaku berlubang** didorong di depan **cahaya alami** →
**LDR** per nada jadi gerbang optik → osilator **CD40106** (Schmitt) → mixer pasif → tone →
volume → **LM386** → jack aux 3.5mm. Handheld, engkol tangan, baterai 9V.
Konsep *Sun Organ*: cahaya alami = ekspresi dinamika (pitch tetap dikunci R·C).

*Poda* dalam bahasa Batak berarti nasihat atau ajaran. Bentuk lipatan akordeon pustaha laklak
dikombinasikan dengan kartu nada organ angin klasik; lubang nada diganti aksara Batak.
Dibuat saat residensi di Bitwäscherei, bersama [Kawan Pustaha](https://kawanpustaha.com).

Repo ini hanya berisi **hasil akhir** (versi 4 channel).

## Status
- 4 channel polifonik BUNYI di PCB etch — osilator + gerbang + amp + jack
- Schematic 4-channel SPICE-validated (`simulation/organ_spice.cir`)
- PCB single-sided, toner-transfer, solder tangan; DRC bersih (`docs/drc4.rpt`)
- Sisa: tuning pentatonik (4 nada, mis. C–D–E–G) + mekanik engkol/roller + casing

## Spesifikasi ringkas (4-channel)
- U1 = CD40106 (osilator, gate 1/2/4/5) · U2 = LM386 (amp)
- LDR1–4 (GL5528) · RV1–4 = trimpot 100k (pitch) · RV5 = 10k (volume)
- C timing = 47nF · R mixer = 10k · Zobel R6 10Ω + C10 0,47µF
- Attenuator R7 1k / R8 4,7k → level-line ke speaker aktif
- Peta gate: ch1 pin1/2 · ch2 pin3/4 · ch3 pin11/10 · ch4 pin9/8

## Isi repo
- `pcb/` — `organ4.kicad_pcb` (KiCad), project, footprint `mylib.pretty`
- `fabrication/` — `organ4_ETCH_bottom_mirrored.svg` (**artwork cetak toner-transfer, 1:1, mirror**), layout jumper
- `docs/` — panduan build + checklist + urutan tes (`BUILD_GUIDE_board_baru.md`), BOM, legend, netlist, gambar perakitan
- `simulation/` — model SPICE (ngspice) dan hasilnya

## Lisensi
Proyek pribadi / eksperimen. Gunakan atas risiko sendiri.

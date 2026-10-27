# LEGEND — Optical Card Organ (4 channel)
Peta kode komponen (reference designator) → apa & fungsinya.

## Kunci awalan (prefix)
| Kode | Arti |
|---|---|
| **LDR** | Light Dependent Resistor (sensor cahaya / fotoresistor) |
| **RVp** | Resistor Variable (trimmer) — **p = pitch** |
| **RVvol** | Resistor Variable — **volume** (potensiometer) |
| **Ct**  | Capacitor timing (penentu nada osilator) |
| **C…**  | Capacitor (kapasitor) lain |
| **Rmix / Rbleed / Rz / Ratt** | Resistor (mixer / bleed / Zobel / attenuator) |
| **U**   | IC (integrated circuit) |
| **J**   | Konektor / jack |

## Per channel (ada 4: ganti angka 1–4)
| Kode | Nilai | Komponen | Fungsi |
|---|---|---|---|
| **LDR1–4** | GL5528 | Fotoresistor | Gerbang optik tiap nada — terang = bunyi, gelap = senyap |
| **RVp1–4** | 100k | Trimmer | **Setel PITCH / nada** tiap channel (mis. RVp4 = tuning nada ke-4) |
| **Ct1–4**  | 47nF | Kapasitor | Kapasitor timing osilator (bareng trimmer menentukan frekuensi) |
| **Rmix1–4**| 10k  | Resistor | Mixer — gabungkan output tiap channel ke SUMMING node |

## Osilator & mixer
| Kode | Nilai | Komponen | Fungsi |
|---|---|---|---|
| **U1** | CD40106 | IC Hex Schmitt inverter | **4 osilator** (1 gerbang = 1 nada) |
| **Rbleed** | 10k | Resistor | Bleed ke GND di titik SUMMING (atur level) |
| **Ctone**  | 10nF | Kapasitor | Tone (low-pass) — haluskan suara kasar square wave |
| **Cdc**    | 1µF | Kapasitor | DC-block sebelum volume (buang tegangan DC) |

## Volume & penguat (audio out)
| Kode | Nilai | Komponen | Fungsi |
|---|---|---|---|
| **RVvol** | 10k | Potensiometer | **Volume** |
| **U3** | LM386 | IC power amp | Penguat / line driver ke jack |
| **Cs1** | 0.1µF | Kapasitor | Decoupling supply LM386 |
| **Cs2** | 10µF | Elko | Reservoir supply LM386 |
| **Cbyp**| 10µF | Elko | Bypass (pin 7 LM386) |
| **Rz**  | 10Ω | Resistor | Zobel network (stabilkan amp) |
| **Cz**  | 0,47µF | Kapasitor | Zobel network |
| **Cout**| 10µF | Elko | DC-block output |
| **Ratt1** | 1k | Resistor | Attenuator (turunkan ke level aux) |
| **Ratt2** | 4,7k | Resistor | Attenuator |

## Konektor
| Kode | Komponen | Fungsi |
|---|---|---|
| **J1** | Header 2-pin | Input baterai 9V (+ / −) |
| **J2** | Jack 3.5mm | Output audio (aux) |

Catatan: master-tune (RVm/Q1/Cosc) TIDAK ada di versi 4-channel — osilator jalan
langsung dari +9V, tuning cukup via RVp1–4. Elko (Cs2, Cbyp, Cout) perhatikan polaritas +/−.

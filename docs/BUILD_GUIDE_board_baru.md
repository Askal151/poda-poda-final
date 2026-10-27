# OPTICAL CARD ORGAN (Sun Organ) — PANDUAN BUILD BOARD BARU
4-channel. Schematic SPICE-validated (BENAR). Rakit 1 channel dulu → test → replikasi.

================================================================
BOM (daftar komponen)
================================================================
IC:
  U1 = CD40106 (hex Schmitt) — osilator 4 channel (pakai gate 1,2,4,5)
  U2 = LM386 — amplifier  [PAKAI SOCKET, chip SEHAT]
Sensor:
  LDR1-4 = GL5528 (4 buah)
Trimpot/pot:
  RV1-4 = 100k trimpot (pitch, 1 per channel)
  RV5   = 10k pot (volume)
Resistor:
  R1-4  = 10k (mixer, 1 per channel)
  R5    = 10k (bleed)
  R6    = 10Ω (Zobel)
  R7    = 1k, R8 = 220Ω (attenuator output)
Kapasitor:
  C1-4  = 47nF (timing, 1 per channel) [karena trimpot 100k]
  C5    = 10nF (tone)
  C6    = 1µF (Cdc / DC-block)          [elektrolit: + ke SUMMING]
  C7    = 0,1µF (supply decoupling, keramik)
  C12   = 0,1µF (decoupling tambahan, dekat U1 CD40106: +9V <-> GND) [pad ada di artwork ornament]
  C13   = 0,1µF (decoupling tambahan, dekat U2 LM386:  +9V <-> GND) [pad ada di artwork ornament]
  C8    = 10µF (supply decoupling)       [elektrolit: + ke +9V]
  C9    = 10µF (bypass pin7)             [elektrolit: + ke pin7]
  C10   = 0,47µF (Zobel, keramik)
  C11   = 10µF (output coupling)         [elektrolit: + ke pin5]
Konektor:
  J1 = baterai 9V ;  J2 = jack aux out 3.5mm
Lain: baterai 9V BARU, kabel, PCB/perfboard, socket IC.

================================================================
POWER
================================================================
 +9V ─┬─ U1 pin14 ─┬─ U2 pin6 ─┬─[C7 0,1µF]─GND
      │            │           └─[C8 10µF +]─GND
 GND ─┴─ U1 pin7   ├─ U2 pin2
                   └─ U2 pin4
 (GND = common: baterai−, semua GND, jack SLEEVE)

================================================================
OSILATOR + GERBANG (×4)  — PALING KRUSIAL, teliti!
================================================================
Per channel:
   pin IN ─┬─ WIPER trimpot (RV, 100k)      <- feedback R variabel
           └─ C timing (47nF) ── GND
   pin OUT ─┬─ UJUNG trimpot (1 ujung saja)
            └─ LDR ── R mixer(10k) ── SUMMING
   (ujung trimpot ke-3 = MENGAMBANG / boleh ke wiper)

PETA GATE (gate 1,2,4,5 dipakai):
   ch1: IN=pin1  OUT=pin2
   ch2: IN=pin3  OUT=pin4
   ch3: IN=pin11 OUT=pin10
   ch4: IN=pin9  OUT=pin8
   NGANGGUR: gate3 IN=pin5, gate6 IN=pin13  -> KE GND
   U1 pin14 -> +9V ; pin7 -> GND

ATURAN OSILATOR (biar berosilasi):
  - pin IN harus BISA naik-turun (cap di-charge via trimpot). JANGAN short ke GND.
  - Ukur pin IN <-> pin OUT: HARUS BERUBAH 0..~100k saat trimpot diputar
    (bukan OL=open, bukan 0=short). Pakai range meter 200k/2M (bukan 20k!).
  - Setel awal ~50k (di range 200k ~0.05) -> nada ~ ratusan Hz.

================================================================
MIXER -> VOLUME -> AMP (LM386)
================================================================
 SUMMING ─┬─ R1..R4 (10k masing2, dari tiap LDR)
          ├─ R5 bleed(10k) ── GND
          ├─ C5 tone(10nF) ── GND
          └─ C6 Cdc(1µF +)── RV5 kaki ATAS

 RV5 (10k): ATAS <- Cdc ; WIPER -> U2 pin3 ; BAWAH -> GND

 U2 (LM386):
   pin2 -> GND     pin3 <- RV5 wiper
   pin4 -> GND     pin6 -> +9V
   pin1,8 = NC (gain x20)
   pin7 -> C9(10µF +) -> GND     [bypass]
   pin5 (OUT):
     ├─ R6(10Ω) ─ C10(0,47µF) ─ GND         (ZOBEL - wajib!)
     └─ C11(10µF +) ─ NODE_B ─ R7(1k) ─ NODE_C ─┬─ R8(220Ω) ─ GND
                                                ├─ jack TIP
                                                └─ jack RING
     jack SLEEVE -> GND

================================================================
CHECKLIST ANTI-GAGAL (semua ini yang merusak board lama)
================================================================
[ ] Osilator: pin IN = wiper trimpot + C47nF(->GND). Feedback IN<->OUT berubah 0-100k.
[ ] Trimpot: WIPER->IN, 1 UJUNG->OUT, ujung ke-3 bebas. (Identifikasi wiper = kaki yg bikin R berubah.)
[ ] LDR sadap pin OUT (2/4/8/10), BUKAN pin IN.
[ ] pin IN JANGAN ke-short GND (47nF cap SEHAT, bukan jebol; tak ada bridge).
[ ] U1 gate nganggur: pin5 & pin13 -> GND.
[ ] LM386 pin5 =/= pin6 (JANGAN jembatan! -> chip jebol+panas). ZOBEL (R6+C10) wajib.
[ ] RV5 wiper -> LM386 pin3 (solid). RV5 bawah -> GND. RV5 atas -> Cdc.
[ ] Cdc (C6) kaki2 -> RV5 atas (jangan putus).
[ ] Polaritas ELEKTROLIT: C9(+->pin7), C8(+->+9V), C6/Cdc(+->summing), C11(+->pin5).
    KEBALIK = short/jebol (C9 kebalik pernah bikin +9V-GND short 1.9ohm).
[ ] GND semua common + jumper (C1<->C2, GND LM386).
[ ] IC pakai yang SEHAT (bukan yg pernah jebol). Socket. Takik/pin1 benar arah.

================================================================
URUTAN RAKIT + TEST (JANGAN rakit semua lalu debug!)
================================================================
TAHAP 1 - 1 osilator:
  Rakit U1 + ch1 (C1, RV1, LDR1 blm perlu). pin14->+9V, pin7->GND, gate nganggur->GND.
  pin1=RV1 wiper + C1(->GND); pin2=RV1 ujung.
  TEST: ukur pin1<->pin2 berubah 0-100k saat RV1 diputar? Set ~50k.
        power on, ukur pin2 DC ~4,5V (BUKAN ~0/9 nyangkut) = BEROSILASI.
        (opsional: pin2 -> cap -> speaker aktif = dengar nada, putar RV1 = pitch berubah)

TAHAP 2 - amp:
  Rakit U2 (LM386) + decoupling + Zobel + output + jack.
  TEST: power on, pin5 DC ~4,5V (setengah supply) = amp bias OK.
        finger test pin3 -> DENGUNG KERAS = amp hidup.

TAHAP 3 - sambung osilator->gate->amp (ch1):
  pin2 -> LDR1 -> R1(10k) -> SUMMING -> C6(Cdc) -> RV5 atas -> wiper -> pin3.
  + R5 bleed(->GND), C5 tone(->GND).
  TEST: sinari LDR1 + volume maks -> BUNYI. Tutup LDR1 -> senyap (gate). RV1 -> pitch.

TAHAP 4 - replikasi ch2,3,4:
  Ulang pola osilator+gate (ganti pin gate sesuai peta, R/C/RV/LDR nomor beda).
  Semua LDR -> R mixer -> SUMMING yang sama.
  Tuning tiap channel pakai app tuner (pentatonik: C-D-E-G-A).

TAHAP 5 - finishing:
  Sekat anti-bleed antar-LDR, apertur, jumper GND, casing.

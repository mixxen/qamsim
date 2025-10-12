# 4‑QAM Interactive Simulator

An interactive, single‑file HTML/JS simulator that visualizes 4‑QAM (QPSK) modulation and demodulation in real time. Enter a bit sequence (pairs of bits), adjust the carrier frequency and noise, and see waveforms, constellation, and SNR update live.

## Features

- Bit sequence input (2 bits per symbol)
- Adjustable carrier frequency `f` (relative to symbol rate)
- Adjustable noise strength `σ` (AWGN)
- Time‑domain plots: `I·cos`, `Q·sin`, combined passband, mixer outputs
- Estimated baseband `I(t)` and `Q(t)` via integrate‑and‑dump per symbol
- Constellation diagram of measured `(Iest, Qest)` with dynamic scaling
- SNR display: measured and theoretical, plus Ps/Pn power readout

## How It Works

### Modulation (Transmitter)

- The input stream is grouped into 2‑bit symbols. Mapping in this simulator is `0 → −1`, `1 → +1` for both I and Q, so `00` is bottom‑left and `11` is top‑right in the constellation.
- Carriers: cosine for I and sine (90° shifted) for Q at frequency `f` (relative to symbol rate).
- Transmitted signal:

  `s(t) = I(t)·cos(2π f t) + Q(t)·sin(2π f t)`

### Demodulation (Receiver)

- Multiply the received signal by local `cos(2π f t)` and `sin(2π f t)` (mixers).
- Integrate over each symbol period (integrate‑and‑dump) to estimate `Iest` and `Qest`.
- With integer `f` (cycles per symbol) the orthogonality is cleanest; non‑integer `f` introduces some leakage.

### Constellation

- Plots the measured `(Iest, Qest)` per symbol.
- Dynamic scaling keeps points visible at high noise; dashed guides mark `I = ±1`, `Q = ±1`.

## SNR and Power

- Measured SNR uses per‑symbol power:
  - `Ps = mean(I^2 + Q^2)` for the ideal symbol vector
  - `Pn = mean((Iest−I)^2 + (Qest−Q)^2)` error power
  - `SNR_linear = Ps / Pn`, `SNR_dB = 10·log10(SNR_linear)`
- Theoretical SNR (for integrate‑and‑dump with AWGN `σ`):
  - `SNR_theory_linear = sps / (2 · σ^2)`
  - `SNR_theory_dB = 10·log10(sps) − 10·log10(2) − 20·log10(σ)`
- Note: In this simulator, samples‑per‑symbol (`sps`) scales with `f` for plotting/estimation, so increasing `f` increases `sps` and the measured/theoretical SNR.

## Controls

- Bit sequence: even length string of `0`/`1` (2 bits per symbol)
- Frequency `f`: non‑negative number (relative to symbol rate)
- Noise strength `σ`: AWGN standard deviation (0 disables noise)

## Run It

- Open `index.html` in any modern web browser.
- Edit the bit sequence or adjust the controls to see real‑time updates.

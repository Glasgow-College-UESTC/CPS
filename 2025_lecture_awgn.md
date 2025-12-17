---
marp: false
title: UESTC 3018 - Lecture 19
description: Digital Modulation in AWGN (Detailed)
theme: uncovered
paginate: true
math: katex
transition: fade
_paginate: false
style: |
  .columns {
    display: grid;
    grid-template-columns: repeat(2, minmax(0,1fr));
    gap: 1rem;
  }
  section {
      background-color: #FFFCEE;
  --color-background: #FFFCEE;
  }
  img[alt~="center"] {
      display: block;
      margin: 0 auto;
  }
  .math-block {
    background-color: #f0f4f8;
    padding: 15px;
    border-radius: 5px;
    border-left: 5px solid #093867;
    font-size: 0.9em;
  }
  h1 { color: #093867; }
  h2 { color: #005f86; }
_backgroundColor: "#FFFCEE"
_color: "#093867"
---

![bg 30% a decorative background](assets/UESTC%203018%203.svg)

---

# UESTC 3018 - Communication Systems

Lecture 19 — Digital Modulation in AWGN Channels
*Bridging the Gap: From Baseband to Passband*

Dr Hasan Abbas
[Hasan.abbas@glasgow.ac.uk](Hasan.Abbas@glasgow.ac.uk)

---

# The "Missing Link": Baseband vs. Passband

**Lecture 18 (Baseband):**
- Signal sits near DC ($0$ Hz).
- $m(t)$ is a pulse stream (Rect, Sinc).
- **Limit:** Cannot propagate through free space (Antenna $\propto \lambda$).

**Lecture 19 (Passband):**
- Signal is shifted to carrier frequency $f_c$.
- **Mechanism:** Multiplication by a sinusoid.
- **Result:** $s(t) = m(t) \cdot \cos(2\pi f_c t)$.



---

# The Modulation Theorem (The Maths)

How do we move frequency? The Fourier Transform property:

$$m(t) \cos(2\pi f_c t) \iff \frac{1}{2} [M(f - f_c) + M(f + f_c)]$$

<div class="math-block">

1.  **Baseband $M(f)$:** Centered at 0 Hz.
2.  **Multiply by $\cos$:** Spectrum splits and shifts to $\pm f_c$.
3.  **Result:** We can now transmit voice ($4$ kHz) over Wi-Fi ($2.4$ GHz).

</div>

**Bandwidth Implication:**
Passband transmission usually requires **Double the Bandwidth** of Baseband ($B_{pass} = 2B_{base}$) because of the two sidebands.

---

# System Model: The "Big Picture"

(Ref: Lecture 18 Notes, Slide 3)

$$\text{Source} \to \text{Encoder} \to \boxed{\text{Modulator}} \to \text{Channel} \to \boxed{\text{Demodulator}} \to \text{Sink}$$

**The Channel (AWGN):**
- The signal is corrupted by **Additive White Gaussian Noise**.
- $r(t) = s_i(t) + n(t)$
- **Goal:** The Demodulator must calculate which $s_i(t)$ was most likely sent, minimizing the Probability of Error ($P_e$).

---

# Geometric Representation (Signal Space)

To solve the detection problem, we stop thinking in "Time" and start thinking in "Vectors".

**Gram-Schmidt Orthogonalization:**
Any set of $M$ signals can be represented as a linear combination of $N$ orthonormal basis functions $\{\phi_j(t)\}$.

$$s_i(t) = \sum_{j=1}^{N} s_{ij} \phi_j(t)$$

- **Vector:** $\mathbf{s}_i = [s_{i1}, s_{i2}]$ (Coordinates in space).
- **Energy:** $E_i = ||\mathbf{s}_i||^2$.

---

# The Basis Functions ($I$ and $Q$)

For most digital modulation, $N=2$. We define an Orthogonal Basis:

<div class="columns">
<div>

**1. In-Phase ($\phi_1$):**
$$\phi_1(t) = \sqrt{\frac{2}{T}} \cos(2\pi f_c t)$$

**2. Quadrature ($\phi_2$):**
$$\phi_2(t) = \sqrt{\frac{2}{T}} \sin(2\pi f_c t)$$

</div>
<div>

**Orthogonality Condition:**
$$\int_0^T \phi_1(t) \phi_2(t) dt = 0$$

*This ensures the I and Q channels do not interfere with each other.*

</div>
</div>

---

# 1. Binary Phase Shift Keying (BPSK)

We map 1 bit $\to$ 2 symbols.
The carrier phase is shifted by $180^\circ$ (antipodal).

**Signal Definition:**
$$s_1(t) = \sqrt{\frac{2E_b}{T_b}} \cos(2\pi f_c t) \quad (\text{Logic 1})$$
$$s_2(t) = -\sqrt{\frac{2E_b}{T_b}} \cos(2\pi f_c t) \quad (\text{Logic 0})$$

**Vector Coordinates:**
- $s_1 = [+\sqrt{E_b}, 0]$
- $s_2 = [-\sqrt{E_b}, 0]$

---

# BPSK Performance

<div class="columns">
<div>



**Decision Boundary:**
- The vertical axis ($I=0$).
- If received $r > 0$, decide "1".
- If received $r < 0$, decide "0".

</div>
<div>

**Probability of Error ($P_e$):**
Depends on the Euclidean distance $d_{12} = 2\sqrt{E_b}$.

$$P_e = Q\left( \sqrt{\frac{2E_b}{N_0}} \right)$$

*Note: BPSK is the most robust PSK modulation.*

</div>
</div>

---

# 2. Quadrature PSK (QPSK)

To increase efficiency, we use **both** basis functions ($\phi_1$ and $\phi_2$).

**Signal Definition:**
$$s_i(t) = \sqrt{E} \cos(2\pi f_c t + \theta_i), \quad \theta_i \in \{ \frac{\pi}{4}, \frac{3\pi}{4}, \frac{5\pi}{4}, \frac{7\pi}{4} \}$$

**Trigonometric Expansion (The Hardware View):**
$$s_i(t) = \underbrace{a_I \cos(2\pi f_c t)}_{\text{I-Channel}} - \underbrace{a_Q \sin(2\pi f_c t)}_{\text{Q-Channel}}$$

We transmit **2 bits** simultaneously: one on Cosine, one on Sine.

---

# QPSK Constellation & Bandwidth

<div class="columns">
<div>

**Constellation:**
- 4 points at $(\pm \sqrt{E/2}, \pm \sqrt{E/2})$.
- **Gray Coding:** Adjacent points differ by only 1 bit.

**Spectral Efficiency:**
- QPSK sends 2 bits/symbol.
- It uses the **same bandwidth** as BPSK.
- **Result:** Double the Data Rate for free.

</div>
<div>



</div>
</div>

---

# 3. M-ary QAM (Quadrature Amplitude Modulation)

Phase modulation has limits. To go faster, we vary **Amplitude** too.

**Signal Definition:**
$$s_i(t) = a_i \phi_1(t) + b_i \phi_2(t)$$
where $a_i, b_i$ are amplitude levels (e.g., $\pm 1, \pm 3$).

**16-QAM:**
- 4 levels on I-axis, 4 levels on Q-axis.
- $4 \times 4 = 16$ symbols.
- **4 bits per symbol.**

---

# The Power/Bandwidth Trade-off

As $M$ increases (16-QAM $\to$ 64-QAM):

1.  **Bandwidth Efficiency ($\eta$) Increases:**
    We send more bits per Hertz. $\eta = \log_2 M$.
2.  **Noise Immunity Decreases:**
    Points are packed tighter together.
3.  **Power Requirement Increases:**
    To maintain the same Error Rate, we must increase Transmit Power to push the points apart.

> **Engineering Choice:** High SNR channel? Use 64-QAM. Noisy channel? Use BPSK.

---

# <span style="color:white">Part 3: Handling Wideband Channels (OFDM)</span>

![bg opacity:100%](assets/gradient3.png)

---

# The Limitation of Single-Carrier

So far, we assumed a simple channel.
**Reality:** High-speed channels have **Multipath Fading**.

- **Delay Spread:** Reflections arrive at different times.
- **Result:** Inter-Symbol Interference (ISI).
- If Symbol Time $T_s < \text{Delay Spread}$, the symbols crash into each other.

**Traditional Fix:** Complex Equalizers (Hard to build for high speeds).

---

# The Solution: OFDM

**Orthogonal Frequency Division Multiplexing**

Instead of sending 1 stream at 100 Mbps (Short $T_s$), we send **100 streams** at 1 Mbps (Long $T_s$).

**Key Mechanism:**
1.  **Serial-to-Parallel:** Split bits into $N$ sub-streams.
2.  **IFFT:** Modulate them onto $N$ orthogonal sub-carriers.
3.  **Result:** Each sub-channel is "narrowband" and flat. ISI is negligible.



---

# Comparison: Single vs Multi-Carrier

| Feature | Single Carrier (QAM) | Multi-Carrier (OFDM) |
| :--- | :--- | :--- |
| **Symbol Duration** | Very Short (High ISI) | Long (Low ISI) |
| **Equalization** | Complex Time-Domain EQ | Simple 1-tap Freq EQ |
| **Fading** | Whole signal fades | Only some sub-carriers fade |
| **Hardware** | Linear Amplifier needed | FFT/IFFT needed |

*OFDM is the engine behind 4G, 5G, and Wi-Fi.*

---

# Summary

1.  **Modulation:** Frequency shifting via $m(t) \cdot \cos(\omega_c t)$ allows wireless transmission.
2.  **Signal Space:** We use **I/Q vectors** to analyze performance. Distance = Robustness.
3.  **QPSK:** The "sweet spot" of modulation (2x rate of BPSK with same bandwidth).
4.  **OFDM:** The modern solution for high-speed data, replacing complex equalizers with parallel sub-carriers (IFFT).

---

# Questions ❓
- You can ask on Menti

---

# Further Reading

- **Lecture 18 Notes (Afsaneh)**: Focus on Slides 6-25.
- **Lathi Chapter 6:**
  - 6.2 (Signal Space)
  - 6.6 (QAM)
  - 6.8 (OFDM)

---

# Get in touch

Hasan.Abbas@glasgow.ac.uk
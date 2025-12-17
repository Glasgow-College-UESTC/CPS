---
marp: false
title: UESTC 3018 - Communication Systems and Principles
description: Course Slides for the CPS course
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
_backgroundColor: "#FFFCEE"
_color: "#093867"
---


<!-- _header: ![h:5em](assets/UoG_keyline.svg) -->

# UESTC 3018 - Communication Systems and Principles

Lecture 18 — From Bits to Symbols, Baseband to Passband

Dr Hasan Abbas
[Hasan.abbas@glasgow.ac.uk](Hasan.Abbas@glasgow.ac.uk)
<!-- transition: fade -->
<!-- <style scoped>a { color: #eee; }</style> -->

<!-- This is presenter note. You can write down notes through HTML comment. -->

---

# From Last Time ⌛

- Quantisation

---

# Today's Lecture 📆

- Line Coding
- Pulse Code Modulation
- Delta Modulation
- Inter-Symbol Interference

![bg right:65% 90%](assets/digital_comms.jpeg)

---


# <!--fit--> <span style="color:white">Baseband Transmission</span>

![bg opacity:100%](assets/gradient3.png)

---

# Encoding & Line Coding

- We have a logical sequence: `1 0 1 1 0`.

- A wire (medium) doesn't understand (digital) logic; it only understands <span style="color:red">**voltage**</span>.

**Line Coding** is the mapping of logical bits to physical waveforms.

### Key Design Goals

1.  <span style="color:red">Synchronisation</span> The receiver needs to recover the clock.
2.  **Bandwidth Efficiency:** Fit more data in less Hz.
3.  **DC Balance:** Ideally zero DC (transformers block DC).

---

## Line Coding Schemes (RZ)

| Scheme           | Signal Levels | Features                                                                                                            |
| :--------------- | :------------ | :------------------------------------------------------------------------------------------------------------------ |
| **On-Off (RZ)**  | $V, 0$        | High $V$ for half bit, then 0. Simple, clear transitions, requires more bandwidth.           |
| **Polar (RZ)**   | $+V, 0, -V$   | $+V$ for half bit (1), $-V$ for half bit (0). Eliminates DC component, good synchronisation. |
| **Bipolar (RZ)** | $+V, 0, -V$   | Alternates between $+V$ and $-V$ for 1s. No DC component, good error detection.              |

---

# Common Line Codes

### 1. NRZ (Non-Return to Zero)
- `1` = $+V$, `0` = $-V$.
- 🙂 Simple, low bandwidth.
- 🙁 Long strings of 1s cause loss of sync (DC buildup).

### 2. Manchester Encoding
- `1` = High $\to$ Low; `0` = Low $\to$ High.
- 🙂Guaranteed transition every bit (Great Sync).
-  🙁 Uses **2x Bandwidth**.


![bg right:45% 80% line codes](assets/code2.svg)

---

# Pulse Code Modulation (PCM)

PCM is the standard for uncompressed digital audio (CDs etc).

- Bit Rate ($R_b$):
$$R_b = n \times f_s$$

- Dynamic Range (SQNR):
$$\text{SQNR}_{dB} \approx 4.8 + 6n$$

![bg right:45% 95%](assets/PCM_block.svg)

---

# PCM Bandwidth

Digital signals require significantly more bandwidth than the original analog signal.

- Analog Voice: $4 \text{ kHz}$.
- PCM Voice (8 bits, 8 kHz): $64 \text{ kbps}$.

- Minimum Transmission Bandwidth:
$$BW_{PCM} \ge \frac{1}{2} R_b = \frac{1}{2} n f_s$$

We trade Bandwidth for Noise Immunity and Regenerability.

---

# Delta Modulation (DM)

PCM sends the *absolute* value of every sample.

- 💡 Adjacent samples are usually similar (high correlation). Why send the whole value?

- Only transmit the change (slope) from the last sample.
- 1 bit per sample:
-  `1`: Signal went UP ($+\Delta$).
- `0`: Signal went DOWN ($-\Delta$).

- Extremely simple hardware (1-bit ADC).

---

# Inter-Symbol Interference (ISI)

Real channels act like Low Pass Filters. They "smear" pulses.
- The "tail" of one pulse spills into the next time slot.
- This is **ISI**. It ruins our ability to distinguish `1` from `0`.

<div class="columns">
<div>

**Ideal Pulse:**
Square (Requires $\infty$ Bandwidth).

**Real Pulse:**
Rounded and spread out.

</div>
<div>

![h:250 center](assets/isi.svg)

</div>
</div>

---

# The Eye Diagram

![h:500 center](assets/eye_diagram.png)

---

# Visualising ISI: The Eye Diagram

If we overlay thousands of received bit periods on an oscilloscope, we get an **Eye Diagram**.

- Vertical Opening (Height): Noise Margin. How much noise can we tolerate?
- Horizontal Opening (Width): Jitter Margin. How sensitive is the timing?
- Closed Eye: The system has failed. ISI is dominant.

---

# The Nyquist Criterion

How do we eliminate ISI without infinite bandwidth?
**Nyquist's First Criterion:**
Find a pulse $p(t)$ such that $p(nT_b) = 0$ for all $n \neq 0$.

**The Sinc Pulse:**
$$p(t) = \text{sinc}\left(\frac{t}{T_b}\right)$$
- Bandwidth $= R_b/2$ (Minimum possible).
- Hard to generate, sensitive to timing errors.
- Raised Cosine Filter (Trade a little bandwidth for robustness).

---

# The Guard Band

Nyquist says we *can* transmit at $B = R_b/2$.
- We cannot build perfect "Brick Wall" filters to separate these signals.

- Real filters have a gradual "roll-off" (slope).
- If we pack channels too tightly, their "tails" overlap.
- Adjacent Channel Interference (ACI).

We deliberately leave unused spectrum between channels.

![bg right:35% 100%](assets/guard.svg)

---

# Further Reading 

- Sections 6.1 - 6.3, and 6.6
<span style="color:green">Modern Digital and Analog Communication Systems</span>, $5^{th}$ Edition
- B P Lathi and Zhi Ding

---

# Get in touch

Hasan.Abbas@glasgow.ac.uk 
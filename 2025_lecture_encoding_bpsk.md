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

![bg 30% a decorative background](assets/UESTC%203018%203.svg)

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

# You are the youngest today!

<!-- Import Hanzi Writer HTML Stuff -->

<style>
  .hanzi-word-container {
    display: flex;              /* This aligns children in a row */
    justify-content: center;    /* This centers the row on the slide */
    gap: 10px;                  /* Optional: adds space between characters */
  }
</style>

<div class="hanzi-word-container">
  <div id="hanzi-char-1"></div>
  <div id="hanzi-char-2"></div>
  <div id="hanzi-char-3"></div>
  <div id="hanzi-char-4"></div>
</div>

<script src="https://cdn.jsdelivr.net/npm/hanzi-writer@3.7.2/dist/hanzi-writer.min.js"></script>

<script>
  const writerOptions = {
    width: 250,
    height: 150,
    padding: 5,
    showCharacter: false, // Make character invisible initially
    showOutline: false,   // Make outline invisible initially
    strokeAnimationSpeed: 1.5,
    delayBetweenStrokes: 50,
    radicalColor: '#093867' // glasgow
  };

  const writer1 = HanziWriter.create('hanzi-char-1', '亡', writerOptions);
  const writer2 = HanziWriter.create('hanzi-char-2', '羊', writerOptions);
  const writer3 = HanziWriter.create('hanzi-char-3', '补', writerOptions);
  const writer4 = HanziWriter.create('hanzi-char-4', '牢', writerOptions);

  // Use an async function for a cleaner animation loop
  // 水滴石穿
  // water dripping away wears away stone
  // shui di shi chuan
  async function animateAndLoop() {
    // Loop forever
    while (true) {
      // Animate each character in sequence, waiting for each to finish
      await writer1.animateCharacter();
      await writer2.animateCharacter();
      await writer3.animateCharacter();
      await writer4.animateCharacter();

      // Pause for 5 seconds to show the completed word
      await new Promise(resolve => setTimeout(resolve, 10000));

      // Hide the characters again to prepare for the next loop
      writer1.hideCharacter();
      writer2.hideCharacter();
      writer3.hideCharacter();
      writer4.hideCharacter();
    }
  }

  animateAndLoop();
</script>

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

# The Bandwidth of Line Codes

To understand bandwidth, we look at the <span style="color:green">**Power Spectral Density (PSD)**</span>. This depends on two things:

1. The Shape of the Pulse ($P(f)$):
- A sharp square pulse has a wide frequency spread.
2. The Pattern of the Bits ($R_k$):
-  Do 1s and 0s follow a pattern, or are they random?

$$
S(f) = \underbrace{\frac{1}{T_b} |P(f)|^2}_{\text{Shape}} \times \underbrace{\sum R_k e^{-j\dots}}_{\text{Pattern}}
$$

---

# For Random Data ...

In real data (compressed audio/video), bits are <span style="color:green">random</span> (like coin flips).

1. No Patterns ($R_k = 0$):
- Knowing bit $N$ tells us nothing about bit $N+1$.
- Because there is no pattern, the complex sum $\sum (\dots)$ vanishes!

1. Average Power ($\sigma^2 \to A^2$):
- For a signal swinging between $+A$ and $-A$, the "Variance" is just the amplitude squared.
- Variance = Power = $A^2$.

---

# Derivation: NRZ Bandwidth

For NRZ, we use a rectangular pulse.
- Pulse Shape: $P(f) = T_b \text{sinc}(f T_b)$
- Pattern: Random ($R_k=0$, sum disappears).

The PSD becomes just the Pulse Shape squared $\times$ Power:

$$S_{NRZ}(f) = A^2 T_b \text{sinc}^2(f T_b)$$

### Observation
- The first null is at $f = R_b$.
- Most power is packed in the low frequencies (Baseband).

![bg right:40% 90%](assets/psd_nrz.svg)


---

# Manchester Bandwidth

Manchester uses a "split" pulse ($+V$ then $-V$).
$$P(f) = T_b \text{sinc}\left(\frac{f T_b}{2}\right) \sin\left(\frac{\pi f T_b}{2}\right)$$

$$S_{Manc}(f) = A^2 T_b \text{sinc}^2\left(\frac{f T_b}{2}\right) \sin^2\left(\frac{\pi f T_b}{2}\right)$$

### Observation
1. $\sin(0) = 0 \to$ **Zero DC**.
2. The null is pushed to $f = 2/T_b = 2R_b$.
3. Manchester requires **2x Bandwidth**.


![bg right:40% 90%](assets/manchester_psd.svg)

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

# Equalisation

Nyquist pulses work in *ideal* channels. Real channels add their own filtering $H_c(f)$, causing ISI.

$$Y(f) = X(f)H_c(f) + \text{Noise}$$

We insert an **Equaliser** filter $H_{eq}(f)$ at the receiver to reverse the damage.

### Zero Forcing Equaliser

$$H_{eq}(f) = \frac{1}{H_c(f)}$$

- Ideally restores the signal, but may amplify noise.

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

# 📝 Example

A band-limited voice signal has a maximum frequency $f_m = 3.4 \text{ kHz}$ and maximum amplitude $A_m$. We need to digitise this signal using a guard band of $1.2 \text{ kHz}$.

1. Determine Nyquist Rate:
$$f_N = 2 \times f_m = 2 \times 3.4 \text{ kHz} = 6.8 \text{ kHz}$$

2. Compute Actual Sampling Frequency ($f_s$):
$$f_s = f_N + \text{Guard Band}$$
$$f_s = 6.8 \text{ kHz} + 1.2 \text{ kHz} = \mathbf{8.0 \text{ kHz}}$$

This is why the global standard for digital telephony (PCM) is **8 kHz**. It accommodates the 3.4 kHz voice signal plus a necessary safety margin for real-world filters.

---


# Questions ❓
- You can ask on Menti
<!-- 
<!-- Need to change the QR code here -->

---

# Further Reading 

- Sections 6.1 - 6.3, and 6.6
<span style="color:green">Modern Digital and Analog Communication Systems</span>, $5^{th}$ Edition
- B P Lathi and Zhi Ding

---

# Get in touch

Hasan.Abbas@glasgow.ac.uk 
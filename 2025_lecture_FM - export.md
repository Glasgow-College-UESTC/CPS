---
marp: false
title: UESTC 3018 - Communication Systems and Principles
description: Course Slides for the CPS course
theme: uncovered
paginate: true
transition: fade
_paginate: false
style: |

  .columns {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1rem;
  }
_backgroundColor: "#FFF"
_color: "#093867"

---

<!-- _header: ![h:5em](assets/UoG_keyline.svg) -->

# UESTC 3018 - Communication Systems and Principles

Lecture 11 — Angle Modulation and Zero Crossings

Dr Hasan Abbas
[Hasan.abbas@glasgow.ac.uk](Hasan.Abbas@glasgow.ac.uk)
<!-- transition: fade -->
<!-- <style scoped>a { color: #eee; }</style> -->

---

# Reflection: The Limits of AM 🤔

- Why Modulation?
- Amplitude Modulation (AM) is a linear process
- Information in the amplitude (envelope)
- Noise adds to the amplitude
- AM is <span style="color:red">**not**</span> resilient to noise
- AM wave is a band limited signal

---

# <!--fit--> 💡 <span style="color:white"> Hide Information in the Timing ... </span>

![bg opacity:100% a decorative background](assets/gradient2.jpg)

---

# First, AM ... 🎗

- For an AM signal, $m(t) \cos(2\pi f_c t)$
- The envelope changes along the real axis with $m(t)$
- Or $m(t)$ scales the carrier signal $\cos(2\pi f_c t)$

![bg right:50% 100% an animated figure showing AM phasor](assets/AM_phasor.gif)
![bg right:50% 100% an animated figure showing FM phasor](assets/phaser_animated.gif)

---

# Now ... 🎗️

- Include $m(t)$ as the <span style="color:red">**phase**</span> of the carrier

$$
2 \cos \left(2 \pi f_c t+k m(t)\right)  = e^{j\left(2 \pi f_c t+k m(t)\right)}+e^{-j\left(2 \pi f_c t+k m(t)\right)}
$$

![bg right:50% 100% an animated figure showing FM phasor](assets/FM_phasor.gif)
![bg right:50% 100% an animated figure showing FM phasor](assets/FM_animated.gif)

---

# Instantaneous Frequency 😵‍💫

- In high school, we learnt <span style="color:red">**speed**</span>  $v(t)$ is the time derivative of distance, $x(t)$
- For signals, <span style="color:green">**frequency**</span>, $\omega(t)$ is the time derivative of phase, $\theta(t)$.**
- Time derivatives describe the **instantaneous** phenomenon
- We can vary the <span style="color:orange">*instantaneous frequency*</span> of the signal through $m(t)$

![bg right:50% 100% an animated figure showing FM phasor](assets/speedometer.gif)

---

# <!--fit--> <span style="color:white"> Angle Modulation </span>

![bg opacity:100% a decorative background](assets/gradient2.jpg)

---


# Angle Modulation 🔑

- Keep the modulated wave amplitude the same
- We can modulate a carrier by changing the <span style="color:red">**phase**</span> and <span style="color:green">**frequency**</span>
- Former is PM and latter is FM
- Angle Modulation is much more complex than AM
- A non-linear process

---

# Instantaneous Frequency 😵‍💫

- Remember instantaneous velocity from high school days
- Use of derivatives to describe the **instantaneous** phenomenon
- We can vary the <span style="color:orange">*instantaneous frequency*</span> of the signal through $m(t)$

![bg right:50% 100% concept of instantaneous frequency](assets/inst_freq.svg)

---

# Instantaneous Frequency (contd.) 😣

- At any time instant $t_0$, we can find the instantaneous frequency by taking the derivative
![bg right:50% 100% concept of instantaneous frequency](assets/inst_freq_slope.svg)

---

# A Generalised Sinusoid

Lets look at,
$$
\varphi(t) = A_c \cos \theta (t)
$$

Here $\theta(t)$ is the generalised angle that can have any value

- For a true sinusoidal signal,

$$
\theta(t) = \omega_c t + \theta_0
$$

- $\theta(t)$: **Total Instantaneous Phase**
- $\theta_0$: **Phase Deviation** (carries the message)

---

# Finding the Instantaneous Frequency

- The derivative of the generalised angle,

$$
\omega_i(t) = \frac{d\theta}{dt} = \theta'(t)
$$

- The other way, phase is the integral of the instantaneous frequency,

$$
\theta (t) = \int_{-\infty}^{t} \omega_i(u) du = \theta_0 + \int_{0}^{t} \omega_i(u) du
$$

- We can modulate by either varying $\omega_i(t)$ or $\theta(t)$

---

# Phase Modulation (PM)

- As the name suggests, we vary the phase $\theta(t)$ <span style="color:red">**linearly**</span> with $m(t)$

$$
\theta^{PM}(t) = \omega_c t + k_p m(t)
$$

- The generalised or in fact the transmitted signal is thus,

$$
\varphi^{\mathrm{PM}}(t) = A_c \cos (\omega_c t + k_p m(t))
$$

We get the instantaneous frequency as,
$$
\omega_i^{PM}(t) = \frac{d\theta}{dt} = \omega_c + k_p \dot{m}(t)
$$

- 🔑 $\omega_i^{PM}(t)$ is proportional to the rate of change of $m(t)$
- Rapid changes in $m(t)$ results in higher value of $\dot{m}(t)$
- ❓ What about the bandwidth of the signal?

---

# Frequency Modulation (FM)

- In FM, we vary the frequency $\omega(t)$ <span style="color:red">**linearly**</span> with $m(t)$,

$$
\omega_i^{FM}(t) = \omega_c +k_f m(t)
$$

- The phase $\theta^{FM}$ is,

$$
\theta^{FM}(t) = \int_{-\infty}^{t} \omega_i^{FM}(u) du = \omega_c t + k_f \int_{-\infty}^{t} m(u) du
$$

The transmitted signal is thus,
$$
\varphi^{\mathrm{FM}}(t) = A_c \cos \left(\omega_c t + k_f \int_{-\infty}^{t} m(u) du \right)
$$

Note the parameter $k_f$ which along with $m(t)$ determines the bandwidth

- 🔑 $\theta^{FM}(t)$ to a phase modulator $\equiv$ $\omega_i^{PM}(t)$ to a frequency modulator.

<!-- The integral term needs to be broken into limits from -\infty to zero which can be safely considered to be zero for a causal signal. This leaves only 0 to t limits for the integral. -->
<!-- This also needs to be included with the fact the system is linear at least the part where the frequency varies linearly with the message signal -->
---

# <!--fit--> <span style="color:white"> 📻 FM vs PM </span>

![bg opacity:100% decorative background](assets/gradient2.jpg)

---

# Guess the 🟡, 🔴 and 🟢 signals

![bg right:50% 60% difference in waveforms](assets/Dfference.svg)

---

![bg 60% a block diagram of difference in techniques](assets/Difference.svg)

---

# Some Observations

- Regardless of PM or FM, angle modulation signal has constant amplitude
- Power of the signal remains the same irrespective of $k_p$ or $k_f$.
- PM is a $90\degree$ shifted version of FM and vice versa.

![bg right:50% 100% modulated signals animated](assets/modulation_animated.gif)

---

# An Example

- Given a signal $m(t)$, sketch the FM and PM waves.
- $k_f = 2 \pi \times 10^{5}$, $k_p = 10\pi$ and $f_c = 100$ MHz.

![bg right:60% 90% an example showing a signal](assets/example_signal.svg)

---


# Further Reading

- Section 4.5 - Angle Modulation
<span style="color:green">Modern Digital and Analog Communication Systems</span>, $5^{th}$ Edition
- B P Lathi and Zhi Ding

---

# <!--fit--> <span style="color:white"> 💡 Modulation is the art of transforming simple signals into complex forms! </span>

![bg opacity:100% a decorative background](assets/gradient2.jpg)

---

# Get in touch

<Hasan.Abbas@glasgow.ac.uk>

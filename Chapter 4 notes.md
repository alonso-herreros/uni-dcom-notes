## Digital Communications <!-- omit in toc -->

# Chapter 4: Multipulse Modulation

*Academic year 2024-2025*  

---

* [Introduction](#introduction)
* [Spread Spectrum (SS)](#spread-spectrum-ss)

---

<!--
$$
g(t) → s(t) = ∑_n A[n] g(t-nT)
$$
-->

## Introduction

Multipulse modulations are the most used in the real world. We will no longer be
working in baseband, but in **bandpass** ($ω_c≠0$), and the channels will
usually not be flat, but instead have a frequency response with *fadings*
(inverse spikes in the frequency response). Since avoiding these fadings is not
practical, we'll have to deal with them in other ways, designing a
communications system capable of working despite them.

> **:information_source: Note**
>
> These fadings are usually caused by the environment, due to the fact that the
> signal doesn't reach the receiver directly, but instead bounces off walls,
> floors, etc. This causes the signal to arrive at the receiver through
> different paths, with different delays, and thus different phases. These
> channels are called **multipath channels**.

We have several options:

* **Spread spectrum**: We'll widen the bandwidth by a factor of $N$ in order to
  make the fading impact a smaller portion of our signal. Keep in mind that the
  transimssion rate in this case does not increase by this factor, despite the
  bandwidth increasing. In fact, if the available bandwidth is limited, the
  "original" bandwidth will have to be reduced so that when it's multiplied by
  $N$, it fits in the available bandwidth. Frequency hopping is done in the
  scope of spread spectrum modulation.

* **Multicarrier modulation**: We'll divide the available bandwidth into $N$
  subbands, and we'll transmit the signal in each of these subbands, having each
  subband carry a portion of the signal.

## Spread Spectrum (SS)

Take the case where we use an RCF filter to provive a symbol rate of $R_s$. Our
occupied bandwidth would be:

$$
R_s(1+α)
$$

If we wanted to use spread spectrum, we'd literatlly spread the signal over a
wider bandwidth, one that is $N$ times wider. This means that the occupied
bandwidth would be

$$
NR_s(1+α)
$$

Let's see how this affect the time domain.

Let our previous modulation be one that transmits one symbol every $T$ seconds.
Since we're multiplying the bandwidth by $N$, we're transmitting at a frequency
$N$ times higher, meaning we should be transmitting the same information in
$T/N$ seconds. This period will be called **chip time** $T_c$.

Since we're not changing the symbol rate, we'll actually use the whole $T$
seconds to transmit the same information, but we'll be transmitting it $N$
times.

The way we do this is by defining a shaping filter $g(t)$ in the following way:

* Our repeated signal will be $g_c(t)$
* Each instance of this repeated signal will be multiplied by a coefficient
  $x[m]$

$$
g(t) = ∑_m^{N-1} g_c(t-\frac{T}{N}m)x[m]
$$

Let's give names to some of these new terms:

**Chip time**
: $T_c = \frac{T}{N}$  
  The time period in which we transmit the same information $N$ times.

**Shaping filter at chip time**
: $g_c(t)$  
  The shaping filter that we'll use to transmit the signal in the chip time.
  This will usually be a square root raised cosine filter.

**Spreading sequence**
: $\{x[m]\}_{m=0}^{N-1} ,\; x[n] ∈ \Complex$  
    The sequence of coefficients that we'll use to multiply the chip signal.

Our shaped signal $s(t)$ will be:

$$
\begin{aligned}
    s(t) &= ∑_n A[n] g(t-nT) \\
    &= ∑_n A[n] ∑_{l=0}^{N-1} x[l] g_c(t-lT -nT_c) \\
    &= ∑_n A[n] ∑_{m=nN}^{(n+1)N} x[m-nN] g_c(t-mT_c) \\
\end{aligned}
$$

Its PSD will be:

$$
S_s (jω) = \frac{1}{T} E_s |G(jω)|^2
$$

Where $G(jω)$ is

$$
G(jω) = G_c(jω) ∑_{l=0}^{N-1} x[l] e^{-jωlT_c}
$$

Leaving us with

$$
S_s(jω) = \frac{E_s}{T} |G_c(jω)|^2
    \underbrace{\left|∑_{l=0}^{N-1} x[l] e^{-jωlT_c}\right|^2}_{S_x(eP{jωT_c})}
$$

Where $S_x(e^{jωT_c})$ is the PSD of the spreading sequence. We'll try to make
it as white as possible, so that the PSD of the transmitted signal is flat as
well.

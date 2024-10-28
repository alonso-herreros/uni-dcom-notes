## Digital Communications <!-- omit in toc -->

# Chapter 4: Multipulse Modulation

*Academic year 2024-2025*  

---

<!--
* Spread spectrum modulation
* Multicarrier modulation

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

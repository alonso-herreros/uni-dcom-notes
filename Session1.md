---
date: 2024-09-12
---

# Chapter 1. Introduction

* Baseband vs Bandpass Channels
* Single symbol Tx vs multi-symbol Tx
* BLock diagram of a digital communications system

```mermaid
flowchart LR
src((Source <br/>info))
med{{Pysical <br/>medium}}
dest((Destination))

src --"{0,1}"--> Tx --"x(t)"--> med --"r(t)"--> Rx --"{0,1}"--> dest
```

$$
r(t) = f(x(t)) + n(t) = h(t) * x(t) + n(t)
$$

Where $f(x(t))$ is a linear function

```mermaid
flowchart LR
x(("x(t)")) ---> h["h(t)"] ---> r(("x(t) * h(t)"))
```

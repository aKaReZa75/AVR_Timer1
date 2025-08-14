This document explains **Compare Output Modes** and how they affect the **OC1A** and **OC1B** pins in different timer modes, with practical configuration examples for **normal**, **inverting**, and **non-inverting** outputs.

---

> [!TIP]  
The `x` can be either **A** or **B**, depending on whether you are configuring Channel A (**OC1A**) or Channel B (**OC1B**).

---

### **Compare Output Mode — Non-PWM Mode**

| **COM1x1** | **COM1x0** | **Description**                                              |
|------------|------------|--------------------------------------------------------------|
| 0          | 0          | Normal port operation, OC1x disconnected.                    |
| 0          | 1          | Toggle OC1x on Compare Match.                                |
| 1          | 0          | Clear OC1x on Compare Match.                                 |
| 1          | 1          | Set OC1x on Compare Match.                                   |

---

### **Compare Output Mode — Fast PWM Mode**

| **COM1x1** | **COM1x0** | **Description**                                                     |
|------------|------------|---------------------------------------------------------------------|
| 0          | 0          | Normal port operation, OC1x disconnected.                           |
| 0          | 1          | WGM13:WGM12 = 0: Normal Port Operation. WGM13:WGM12 = 1: Toggle OC1x on Compare Match. |
| 1          | 0          | Clear OC1x on Compare Match, set OC1x at BOTTOM (non-inverting mode). |
| 1          | 1          | Set OC1x on Compare Match, clear OC1x at BOTTOM (inverting mode).   |

> [!NOTE]  
A special case occurs when **OCR1x** equals **TOP** and **COM1x1** is set. In this case, the compare match is ignored, but the set or clear is done at **BOTTOM**.

---

### **Compare Output Mode — Phase Correct / Phase & Frequency Correct PWM Mode**

| **COM1x1** | **COM1x0** | **Description**                                                       |
|------------|------------|-----------------------------------------------------------------------|
| 0          | 0          | Normal port operation, OC1x disconnected.                             |
| 0          | 1          | WGM13:WGM12 = 0: Normal Port Operation. WGM13:WGM12 = 1: Toggle OC1x on Compare Match. |
| 1          | 0          | Clear OC1x on Compare Match when up-counting. Set OC1x on Compare Match when down-counting. |
| 1          | 1          | Set OC1x on Compare Match when up-counting. Clear OC1x on Compare Match when down-counting. |

> [!NOTE]  
A special case occurs when **OCR1x** equals **TOP** and **COM1x1** is set. In this case, the compare match is ignored, but the set or clear is done at **TOP**.

---

### Compare Output Mode — Normal Operation

- Timer used for internal timing only (no waveform on output pins)  
- Suitable for `Normal Mode`, `CTC`, or `PWM` when no output pin is needed

```c
/* Normal Operation (OC1A disconnected) */
bitClear(TCCR1A, COM1A1);
bitClear(TCCR1A, COM1A0);

/* Normal Operation (OC1B disconnected) */
bitClear(TCCR1A, COM1B1);
bitClear(TCCR1A, COM1B0);
```

---

### Compare Output Mode — Toggle

- Generate square waves for frequency testing or simple waveform generation  
- Works in `CTC` mode  
- In `Fast PWM` or `Phase Correct PWM`, only works when `WGM13:WGM12 = 1` (toggle on match with OCRx)

```c
/* Toggle OC1A on Compare Match */
bitClear(TCCR1A, COM1A1);
bitSet(TCCR1A, COM1A0);

/* Toggle OC1B on Compare Match */
bitClear(TCCR1A, COM1B1);
bitSet(TCCR1A, COM1B0);
```

---

### Compare Output Mode — Non-Inverting

- Standard PWM signal generation  
- Output is HIGH at BOTTOM, LOW when TCNT1 = OCR1x  
- Used to drive motors, LEDs, or other analog devices using PWM  
- Very common for controlling power (e.g., speed, brightness)

```c
/* Non-Inverting PWM Output (Clear OC1A on Compare Match) */
bitSet(TCCR1A, COM1A1);
bitClear(TCCR1A, COM1A0);

/* Non-Inverting PWM Output (Clear OC1B on Compare Match) */
bitSet(TCCR1A, COM1B1);
bitClear(TCCR1A, COM1B0);
```

---

### Compare Output Mode — Inverting

- Inverted PWM generation  
- Output is LOW at BOTTOM, HIGH when TCNT1 = OCR1x  
- Used when complementary waveforms are needed (e.g., H-bridge drivers, special dimming effects)

```c
/* Inverting PWM Output (Set OC1A on Compare Match) */
bitSet(TCCR1A, COM1A1);
bitSet(TCCR1A, COM1A0);

/* Inverting PWM Output (Set OC1B on Compare Match) */
bitSet(TCCR1A, COM1B1);
bitSet(TCCR1A, COM1B0);
```


# 🌟 Support Me
If you found this repository useful:
- Subscribe to my [YouTube Channel](https://www.youtube.com/@aKaReZa75).
- Share this repository with others.
- Give this repository and my other repositories a star.
- Follow my [GitHub account](https://github.com/aKaReZa75).

# ✉️ Contact Me
Feel free to reach out to me through any of the following platforms:
- 📧 [Email: aKaReZa75@gmail.com](mailto:aKaReZa75@gmail.com)
- 🎥 [YouTube: @aKaReZa75](https://www.youtube.com/@aKaReZa75)
- 💼 [LinkedIn: @akareza75](https://www.linkedin.com/in/akareza75)

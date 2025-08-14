
This document provides a detailed overview of all waveform generation modes of **Timer1**, including **Normal**, **CTC**, **Fast PWM**, **Phase Correct PWM**, and **Phase & Frequency Correct PWM**, with register configurations and practical examples.

---

## **TCCR1A (Timer/Counter Control Register A)**

| Bit | Description |
|-----|-------------|
| 7   | COM1A1 (Compare Output Mode for Channel A) |
| 6   | COM1A0 (Compare Output Mode for Channel A) |
| 5   | COM1B1 (Compare Output Mode for Channel B) |
| 4   | COM1B0 (Compare Output Mode for Channel B) |
| 3   | Reserved |
| 2   | Reserved |
| 1   | WGM11 (Waveform Generation Mode) |
| 0   | WGM10 (Waveform Generation Mode) |

---

## **TCCR1B (Timer/Counter Control Register B)**

| Bit | Description |
|-----|-------------|
| 7   | ICNC1 (Input Capture Noise Canceler) |
| 6   | ICES1 (Input Capture Edge Select) |
| 5   | Reserved |
| 4   | WGM13 (Waveform Generation Mode) |
| 3   | WGM12 (Waveform Generation Mode) |
| 2   | CS12 (Clock Select) |
| 1   | CS11 (Clock Select) |
| 0   | CS10 (Clock Select) |

---

### **WGM13:WGM12:WGM11:WGM10 — Waveform Generation Mode**

These bits select the waveform generation mode of Timer1. The possible configurations are:

| Mode | WGM13 | WGM12 | WGM11 | WGM10 | Mode Description                  | TOP     | Update of OCRx at | TOV Flag Set on |
|------|-------|-------|-------|-------|-----------------------------------|---------|-------------------|------------------|
| 0    | 0     | 0     | 0     | 0     | Normal                            | 0xFFFF  | Immediate         | MAX              |
| 1    | 0     | 0     | 0     | 1     | PWM, Phase Correct, 8-bit         | 0x00FF  | TOP               | BOTTOM           |
| 2    | 0     | 0     | 1     | 0     | PWM, Phase Correct, 9-bit         | 0x01FF  | TOP               | BOTTOM           |
| 3    | 0     | 0     | 1     | 1     | PWM, Phase Correct, 10-bit        | 0x03FF  | TOP               | BOTTOM           |
| 4    | 0     | 1     | 0     | 0     | CTC                               | OCR1A   | Immediate         | MAX              |
| 5    | 0     | 1     | 0     | 1     | Fast PWM, 8-bit                   | 0x00FF  | BOTTOM            | TOP              |
| 6    | 0     | 1     | 1     | 0     | Fast PWM, 9-bit                   | 0x01FF  | BOTTOM            | TOP              |
| 7    | 0     | 1     | 1     | 1     | Fast PWM, 10-bit                  | 0x03FF  | BOTTOM            | TOP              |
| 8    | 1     | 0     | 0     | 0     | PWM, Phase & Frequency Correct    | ICR1    | TOP               | BOTTOM           |
| 9    | 1     | 0     | 0     | 1     | PWM, Phase & Frequency Correct    | OCR1A   | TOP               | BOTTOM           |
| 10   | 1     | 0     | 1     | 0     | PWM, Phase Correct                | ICR1    | TOP               | BOTTOM           |
| 11   | 1     | 0     | 1     | 1     | PWM, Phase Correct                | OCR1A   | TOP               | BOTTOM           |
| 12   | 1     | 1     | 0     | 0     | CTC                               | ICR1    | Immediate         | MAX              |
| 14   | 1     | 1     | 1     | 0     | Fast PWM                          | ICR1    | BOTTOM            | TOP              |
| 15   | 1     | 1     | 1     | 1     | Fast PWM                          | OCR1A   | BOTTOM            | TOP              |

> [!CAUTION]  
> Mode 13 is reserved and should not be used.

---

## Mode Configuration Examples

---

### **Mode 0: Normal Mode (TOP = 0xFFFF)**

Timer counts from 0 to 65535. Overflow occurs when it wraps around.

```c
/* Configure Timer1 for Normal Mode */
bitClear(TCCR1A, WGM10);
bitClear(TCCR1A, WGM11);
bitClear(TCCR1B, WGM12);
bitClear(TCCR1B, WGM13);
```

---

### **Mode 1: PWM, Phase Correct, 8-bit (TOP = 0x00FF)**

Timer counts up to 255 and back down. Symmetrical PWM waveform.

```c
/* Configure Timer1 for Phase Correct PWM, 8-bit */
bitSet(TCCR1A, WGM10);
bitClear(TCCR1A, WGM11);
bitClear(TCCR1B, WGM12);
bitClear(TCCR1B, WGM13);
```

---

### **Mode 2: PWM, Phase Correct, 9-bit (TOP = 0x01FF)**

Timer counts up to 511 and back down. Higher resolution than 8-bit.

```c
/* Configure Timer1 for Phase Correct PWM, 9-bit */
bitClear(TCCR1A, WGM10);
bitSet(TCCR1A, WGM11);
bitClear(TCCR1B, WGM12);
bitClear(TCCR1B, WGM13);
```

---

### **Mode 3: PWM, Phase Correct, 10-bit (TOP = 0x03FF)**

Timer counts up to 1023 and back down. Even finer resolution.

```c
/* Configure Timer1 for Phase Correct PWM, 10-bit */
bitSet(TCCR1A, WGM10);
bitSet(TCCR1A, WGM11);
bitClear(TCCR1B, WGM12);
bitClear(TCCR1B, WGM13);
```

---

### **Mode 4: CTC (Clear Timer on Compare Match, TOP = OCR1A)**

Timer resets when TCNT1 matches OCR1A. Ideal for precise timing.

```c
/* Configure Timer1 for CTC Mode */
bitClear(TCCR1A, WGM10);
bitClear(TCCR1A, WGM11);
bitSet(TCCR1B, WGM12);
bitClear(TCCR1B, WGM13);
```

---

### **Mode 5: Fast PWM, 8-bit (TOP = 0x00FF)**

Timer counts from 0 to 255. Fast PWM with lower resolution.

```c
/* Configure Timer1 for Fast PWM, 8-bit */
bitSet(TCCR1A, WGM10);
bitClear(TCCR1A, WGM11);
bitSet(TCCR1B, WGM12);
bitClear(TCCR1B, WGM13);
```

---

### **Mode 6: Fast PWM, 9-bit (TOP = 0x01FF)**

Timer counts from 0 to 511. Balanced speed and resolution.

```c
/* Configure Timer1 for Fast PWM, 9-bit */
bitClear(TCCR1A, WGM10);
bitSet(TCCR1A, WGM11);
bitSet(TCCR1B, WGM12);
bitClear(TCCR1B, WGM13);
```

---

### **Mode 7: Fast PWM, 10-bit (TOP = 0x03FF)**

Timer counts from 0 to 1023. High resolution fast PWM.

```c
/* Configure Timer1 for Fast PWM, 10-bit */
bitSet(TCCR1A, WGM10);
bitSet(TCCR1A, WGM11);
bitSet(TCCR1B, WGM12);
bitClear(TCCR1B, WGM13);
```

---

### **Mode 8: Phase & Frequency Correct PWM (TOP = ICR1)**

Timer counts up to ICR1 and back down. Used for center-aligned PWM.

```c
/* Configure Timer1 for Phase & Frequency Correct PWM (TOP = ICR1) */
bitClear(TCCR1A, WGM10);
bitClear(TCCR1A, WGM11);
bitClear(TCCR1B, WGM12);
bitSet(TCCR1B, WGM13);
```

---

### **Mode 9: Phase & Frequency Correct PWM (TOP = OCR1A)**

Timer counts up to OCR1A and back down. Flexible top value.

```c
/* Configure Timer1 for Phase & Frequency Correct PWM (TOP = OCR1A) */
bitSet(TCCR1A, WGM10);
bitClear(TCCR1A, WGM11);
bitClear(TCCR1B, WGM12);
bitSet(TCCR1B, WGM13);
```

---

### **Mode 10: Phase Correct PWM (TOP = ICR1)**

Timer counts up and down with ICR1 as TOP. Center-aligned PWM.

```c
/* Configure Timer1 for Phase Correct PWM (TOP = ICR1) */
bitClear(TCCR1A, WGM10);
bitSet(TCCR1A, WGM11);
bitClear(TCCR1B, WGM12);
bitSet(TCCR1B, WGM13);
```

---

### **Mode 11: Phase Correct PWM (TOP = OCR1A)**

Timer counts up and down with OCR1A as TOP. Adjustable resolution.

```c
/* Configure Timer1 for Phase Correct PWM (TOP = OCR1A) */
bitSet(TCCR1A, WGM10);
bitSet(TCCR1A, WGM11);
bitClear(TCCR1B, WGM12);
bitSet(TCCR1B, WGM13);
```

---

### **Mode 12: CTC (TOP = ICR1)**

Timer resets when TCNT1 matches ICR1. Useful for input capture timing.

```c
/* Configure Timer1 for CTC Mode (TOP = ICR1) */
bitClear(TCCR1A, WGM10);
bitClear(TCCR1A, WGM11);
bitSet(TCCR1B, WGM12);
bitSet(TCCR1B, WGM13);
```

---

### **Mode 14: Fast PWM (TOP = ICR1)**

Timer counts from 0 to ICR1. High-speed PWM with flexible top.

```c
/* Configure Timer1 for Fast PWM (TOP = ICR1) */
bitClear(TCCR1A, WGM10);
bitSet(TCCR1A, WGM11);
bitSet(TCCR1B, WGM12);
bitSet(TCCR1B, WGM13);
```

---

### **Mode 15: Fast PWM (TOP = OCR1A)**

Timer counts from 0 to OCR1A. Fast PWM with adjustable resolution.

```c
/* Configure Timer1 for Fast PWM (TOP = OCR1A) */
bitSet(TCCR1A, WGM10);
bitSet(TCCR1A, WGM11);
bitSet(TCCR1B, WGM12);
bitSet(TCCR1B, WGM13);
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

This document explains how to configure the clock source and prescaler for **Timer1**, including detailed descriptions of all clock select bits and their effects on timer resolution and overflow frequency.

---

## **TCCR1B (Timer/Counter Control Register B)**

| Bit | Description |
|-----|-------------|
| 7   | ICNC1 (Input Capture Noise Canceler) |
| 6   | ICES1 (Input Capture Edge Select)    |
| 5   | Reserved                             |
| 4   | WGM13 (Waveform Generation Mode)     |
| 3   | WGM12 (Waveform Generation Mode)     |
| 2   | CS12 (Clock Select)                  |
| 1   | CS11 (Clock Select)                  |
| 0   | CS10 (Clock Select)                  |

> [!NOTE]  
> **WGM13** and **WGM12** combine with **WGM11** and **WGM10** from TCCR1A to define the waveform generation mode.

---

### **Clock Select Bit Description**

These bits determine the clock source and prescaler for Timer1.

| **CS12** | **CS11** | **CS10** | **Description**                                           |
|----------|----------|----------|-----------------------------------------------------------|
| 0        | 0        | 0        | No clock source (Timer/Counter stopped)                   |
| 0        | 0        | 1        | clk_I/O (No prescaling)                                   |
| 0        | 1        | 0        | clk_I/O / 8 (From prescaler)                              |
| 0        | 1        | 1        | clk_I/O / 64 (From prescaler)                             |
| 1        | 0        | 0        | clk_I/O / 256 (From prescaler)                            |
| 1        | 0        | 1        | clk_I/O / 1024 (From prescaler)                           |
| 1        | 1        | 0        | External clock source on T1 pin. Clock on falling edge.   |
| 1        | 1        | 1        | External clock source on T1 pin. Clock on rising edge.    |

> [!TIP]  
> Use external clocking when Timer1 acts as a **counter** to count external events.  
> Rising or falling edge options allow flexibility in signal synchronization.

---

### **No Clock Source**

```c
/* Stop Timer1 (No clock source) */
bitClear(TCCR1B, CS10);
bitClear(TCCR1B, CS11);
bitClear(TCCR1B, CS12);
```

- Timer completely off  
- Useful for disabling the timer or saving power

---

### **Prescaler = 1**

```c
/* Set Prescaler = 1 => Timer clock = 16MHz */
bitSet(TCCR1B, CS10);
bitClear(TCCR1B, CS11);
bitClear(TCCR1B, CS12);
```

---

### **Prescaler = 8**

```c
/* Set Prescaler = 8 => Timer clock = 2MHz */
bitClear(TCCR1B, CS10);
bitSet(TCCR1B, CS11);
bitClear(TCCR1B, CS12);
```

---

### **Prescaler = 64**

```c
/* Set Prescaler = 64 => Timer clock = 250KHz */
bitSet(TCCR1B, CS10);
bitSet(TCCR1B, CS11);
bitClear(TCCR1B, CS12);
```

---

### **Prescaler = 256**

```c
/* Set Prescaler = 256 => Timer clock = 62.5KHz */
bitClear(TCCR1B, CS10);
bitClear(TCCR1B, CS11);
bitSet(TCCR1B, CS12);
```

---

### **Prescaler = 1024**

```c
/* Set Prescaler = 1024 => Timer clock = 15.625KHz */
bitSet(TCCR1B, CS10);
bitClear(TCCR1B, CS11);
bitSet(TCCR1B, CS12);
```

---

### **External Clock on Falling Edge (T1 Pin)**

```c
/* External Clock Source on Falling Edge */
bitClear(TCCR1B, CS10);
bitSet(TCCR1B, CS11);
bitSet(TCCR1B, CS12);
```

---

### **External Clock on Rising Edge (T1 Pin)**

```c
/* External Clock Source on Rising Edge */
bitSet(TCCR1B, CS10);
bitSet(TCCR1B, CS11);
bitSet(TCCR1B, CS12);
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

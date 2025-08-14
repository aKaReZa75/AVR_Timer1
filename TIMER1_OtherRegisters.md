This file provides a detailed description of **Timer1 data and compare registers**: `TCNT1`, `OCR1A`, `OCR1B`, and `ICR1`. It explains how each register works, including bit-level information and their roles in **compare match** and **input capture** operations for Channel A and B.

---

## **TCNT1 (Timer/Counter Register)**

| Bits | Description              |
|------|--------------------------|
| 15–8 | TCNT1H (High Byte)       |
| 7–0  | TCNT1L (Low Byte)        |

- **TCNT1**: The current value of the Timer/Counter. It increments with each clock cycle (or according to the prescaler).
- Being a 16-bit register, it allows for much longer timing intervals compared to Timer0.

```c
/* Set Timer1 to a specific value */
TCNT1 = 0x1234;
```

---

## **OCR1A (Output Compare Register A)**

| Bits | Description              |
|------|--------------------------|
| 15–8 | OCR1AH (High Byte)       |
| 7–0  | OCR1AL (Low Byte)        |

- **OCR1A**: Holds the value for the output compare match for Channel A.
- When `TCNT1 == OCR1A`, a compare match event occurs.
- In **CTC mode**, OCR1A can define the TOP value of the timer.

```c
/* Set Compare Match value for Channel A */
OCR1A = 0x0FFF;
```

---

## **OCR1B (Output Compare Register B)**

| Bits | Description              |
|------|--------------------------|
| 15–8 | OCR1BH (High Byte)       |
| 7–0  | OCR1BL (Low Byte)        |

- **OCR1B**: Holds the value for the output compare match for Channel B.
- Similar to OCR1A but operates independently on Channel B.
- Useful for generating a second PWM signal or compare match interrupt.

```c
/* Set Compare Match value for Channel B */
OCR1B = 0x07FF;
```

---

## **ICR1 (Input Capture Register)**

| Bits | Description              |
|------|--------------------------|
| 15–8 | ICR1H (High Byte)        |
| 7–0  | ICR1L (Low Byte)         |

- **ICR1**: Captures the value of `TCNT1` when an edge is detected on the **ICP1** pin.
- Used for measuring external signal timing (e.g., pulse width, frequency).
- Can also be used as the TOP value in certain PWM modes.

```c
/* Read captured value */
uint16_t captured = ICR1;
```

> [!TIP]  
Configure edge detection using the `ICES1` bit in `TCCR1B`:  
- `ICES1 = 1` → Rising edge  
- `ICES1 = 0` → Falling edge


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

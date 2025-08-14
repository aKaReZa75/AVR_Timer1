This document helps calculate the **overflow time** and **compare match intervals** in **Normal** and **CTC** modes based on **prescaler** and **system clock**, with practical examples and formulas tailored to Timer1's 16-bit resolution.

---

## **Normal Mode**

The time period for **Timer/Counter 1** to overflow or for a compare match can be calculated as:

```
T = (N * 65536) / f_clk
```

Where:
- `N` is the prescaler value.
- `65536` is the maximum value the timer can count to before overflowing (in **Normal mode**).
- `f_clk` is the system clock frequency.

### **Example Calculation for Time**

For a **16 MHz** clock and a **prescaler of 64**, the time period for a **Timer/Counter 1** overflow would be:

```
T = (64 * 65536) / 16,000,000 = 4,194,304 / 16,000,000 ≈ 0.2621 seconds
```

| **Prescaler** | **Time (ms)** |
|---------------|---------------|
| 1             | 4.096         |
| 8             | 32.768        |
| 64            | 262.144       |
| 256           | 1,048.576     |
| 1024          | 4,194.304     |

This table shows the time period for a **Timer/Counter 1** overflow for different prescaler values, with a **16 MHz** clock and a **max count of 65536** before overflow.

---

## **Time Calculation with TCNT1**

If you want to reach different time intervals in Normal mode, you need to initialize the TCNT1 register to the desired value. After setting TCNT1, you can calculate the time for the timer to reach that specific value before overflowing.

```
T = ((65536 - TCNT1) * N) / f_clk
```

Where:
- `TCNT1` is the current value in the **TCNT1** register (the timer's current count).
- `N` is the prescaler value.
- `f_clk` is the system clock frequency.

#### **Example Calculation for Time Using TCNT1**

If you want the interrupt to occur **1 second** after the current value in **TCNT1**, let's calculate it for a **prescaler of 256** with a **16 MHz** clock.

We want:
- `T = 1 s`
- `N = 256`
- `f_clk = 16,000,000 Hz`

Rearranging the formula:

```
1 = ((65536 - TCNT1) * 256) / 16,000,000
(65536 - TCNT1) = 16,000,000 / 256 = 62500
TCNT1 = 65536 - 62500 = 3036
```

So, you would set **TCNT1 = 3036**.

> [!IMPORTANT]  
You must re-set the TCNT1 register to the calculated value (3036 in this case) at the end of the OVF (overflow) interrupt. This is crucial to maintain timing accuracy.

```c
ISR(TIMER1_OVF_vect)
{
    TCNT1 = 3036;
};
```

---

## **CTC Mode**

In **CTC (Clear Timer on Compare Match)** mode, the timer is cleared when it matches the value in the **OCR1A** register. The period for **Timer/Counter 1** in **CTC mode** can be calculated as:

```
T = (N * (OCR1A + 1)) / f_clk
```

Where:
- `OCR1A` is the value in the **OCR1A** register (the compare match value).
- `N` is the prescaler value.
- `f_clk` is the system clock frequency.

### **Example Calculation for Time in CTC Mode**

For a **16 MHz** clock, a **prescaler of 64**, and **OCR1A = 24999**, the time period would be:

```
T = (64 * (24999 + 1)) / 16,000,000 = (64 * 25000) / 16,000,000 = 1,600,000 / 16,000,000 = 0.1 seconds
```

In **CTC mode**, the timer counts from 0 to **OCR1A** before resetting, which is useful for generating precise periodic interrupts.

> [!TIP]  
In Normal mode, we typically use the **Overflow interrupt (OVF)**.  
In CTC mode, we use the **Compare Match interrupt** to trigger the event when the timer matches the value in **OCR1A**.


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

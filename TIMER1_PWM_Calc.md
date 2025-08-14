This file shows how to calculate the output **PWM frequency** for **Timer1** in both **Fast PWM** and **Phase Correct PWM** modes, using **OCR1A** or **ICR1** as TOP and different **prescaler** settings.

---

## **PWM Phase Correct Mode with `ICR1` or `OCR1A` as TOP**

In **Phase Correct PWM mode**, Timer1 counts from `BOTTOM (0)` to `TOP (ICR1 or OCR1A)` and then back down to `BOTTOM`, forming a symmetrical up-down count. This results in more accurate frequency and duty cycle characteristics, especially important for motor control, servo positioning, or audio PWM applications.

### Formula:

```
f_pwm = f_clk / (2 * N * TOP)
```

Where:
- `f_clk` is the system clock frequency (e.g., 16 MHz),
- `N` is the prescaler,
- `TOP` is the value in `ICR1` or `OCR1A`.

---

### **Example: Generate 50 Hz in Phase Correct PWM mode**

Given:
- `f_clk = 16,000,000 Hz`,
- `Prescaler N = 8`,
- Desired `f_pwm = 50 Hz`.

Use the formula:

```
TOP = f_clk / (2 * N * f_pwm)
```

Plug in the numbers:

```
TOP = 16,000,000 / (2 * 8 * 50)
TOP = 16,000,000 / 800 = 20,000
```

So you would set:

```c
ICR1 = 20000;  /* Generates approximately 50 Hz */
```

> [!NOTE]  
You can also use `OCR1A` as TOP, but using `ICR1` allows you to preserve `OCR1A` and `OCR1B` for duty cycle control.

---

## **Fast PWM Mode with `ICR1` or `OCR1A` as TOP**

In **Fast PWM mode**, Timer1 counts from `BOTTOM (0)` to `TOP`, then resets to zero. It generates a sawtooth waveform which is more efficient for high-frequency PWM applications such as LED dimming or signal generation.

### Formula:

```
f_pwm = f_clk / (N * (TOP + 1))
```

---

### **Example: Generate 10 kHz in Fast PWM mode**

Given:
- `f_clk = 16,000,000 Hz`,
- `Prescaler N = 8`,
- Desired `f_pwm = 10,000 Hz`.

Use the formula:

```
TOP = (f_clk / (N * f_pwm)) - 1
```

Plug in the numbers:

```
TOP = (16,000,000 / (8 * 10,000)) - 1
TOP = (16,000,000 / 80,000) - 1 = 200 - 1 = 199
```

So you would set:

```c
ICR1 = 199;  /* Generates approximately 10 kHz */
```

> [!IMPORTANT]  
Timer1 is a 16-bit timer, so TOP can be up to `65535`. This allows for much finer control and lower frequencies than Timer0.

---

## **Duty Cycle Calculation**

To set the duty cycle using `OCR1A` or `OCR1B`:

```
Duty (%) = (OCR1x / TOP) × 100
```

### Example: OCR1A = 1500, ICR1 = 20000

```
Duty = (1500 / 20000) × 100 = 7.5%
```

This is ideal for servo control, where 7.5% corresponds to the center position.


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

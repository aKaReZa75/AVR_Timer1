
This file explains how to calculate timing and frequency using **Input Capture** with **Timer1**, based on `ICR1` values, system clock, and prescaler. It includes formulas for measuring **pulse width**, **signal period**, and **frequency**, with practical examples and macro usage.

---

## **ICR1 (Input Capture Register)**

| Bits | Description              |
|------|--------------------------|
| 15–8 | ICR1H (High Byte)        |
| 7–0  | ICR1L (Low Byte)         |

- **ICR1** stores the value of `TCNT1` when an edge is detected on the **ICP1** pin.
- Used for measuring external signal timing such as pulse width, frequency, and period.

```c
/* Read captured value */
uint16_t capturedValue = ICR1;
```

> [!TIP]  
Configure edge detection using the `ICES1` bit in `TCCR1B`:  
- `ICES1 = 1` → Rising edge  
- `ICES1 = 0` → Falling edge  
Use:  
```c
bitSet(TCCR1B, ICES1);  // Detect rising edge  
bitClear(TCCR1B, ICES1);  // Detect falling edge
```

---

## **Capture Time Calculation**

To calculate the time between two captured edges:

```
T = (ΔICR1 × N) / f_clk
```

Where:
- `ΔICR1` = ICR1₂ − ICR1₁
- `N` = Prescaler value
- `f_clk` = System clock frequency (e.g., 16 MHz)

### Example: Measure Pulse Width

```c
uint16_t start = 1000;
uint16_t end   = 5000;
uint16_t delta = end - start;  // ΔICR1 = 4000
```

Prescaler = 64  
System Clock = 16 MHz

```
T = (4000 × 64) / 16,000,000 = 256,000 / 16,000,000 = 0.016 s = 16 ms
```

So the pulse width is **16 milliseconds**.

---

## **Frequency Calculation**

To measure the frequency of a signal:

```
f_signal = f_clk / (ΔICR1 × N)
```

### Example: Measure Frequency

```c
uint16_t delta = 4000;
uint16_t prescaler = 64;
```

```
f_signal = 16,000,000 / (4000 × 64) = 16,000,000 / 256,000 = 62.5 Hz
```

So the signal frequency is **62.5 Hz**.

---

## **Interrupt Configuration**

Enable Input Capture interrupt and clear its flag:

```c
bitSet(TIMSK1, ICIE1);       // Enable Input Capture Interrupt
intFlag_clear(TIFR1, ICF1);  // Clear Input Capture Flag
```

Define the ISR:

```c
ISR(TIMER1_CAPT_vect)
{
    uint16_t captured = ICR1;
    // Process captured value
}
```

> [!IMPORTANT]  
Always clear the **ICF1** flag manually using `intFlag_clear(TIFR1, ICF1)` to avoid false triggers.

---

## **Best Practices**

- Use global variables to store previous capture values for delta calculations.
- Use macros like `bitSet`, `bitClear`, and `intFlag_clear` for clean and readable code.
- Configure `ICP1` pin as input:
```c
GPIO_Config_INPUT(DDRB, DDB0);  // ICP1 is on PB0
```
- Enable global interrupts:
```c
globalInt_Enable;
```

---

### 📐 [TIMER1 Capture Calculation](./TIMER1_Capture_Calc.md)  
This file explains how to calculate timing and frequency using **Input Capture** with Timer1, based on `ICR1` values, system clock, and prescaler. It includes formulas for measuring **pulse width**, **signal period**, and **frequency**, along with practical examples and macro usage.

#### 🧮 Example: Calculate Time and Frequency from Tick Count

Suppose you captured two rising edges of a signal using Timer1 and got the following values:

```c
uint16_t ICR1_first  = 12000;
uint16_t ICR1_second = 32000;
```

Then:

```c
uint16_t ticks = ICR1_second - ICR1_first;  // ticks = 20000
```

Assume:
- `f_clk = 16,000,000 Hz` (system clock)
- `Prescaler = 8`

##### ⏱ Time Calculation

```text
Time = (ticks × Prescaler) / f_clk
Time = (20000 × 8) / 16,000,000 = 160,000 / 16,000,000 = 0.01 s = 10 ms
```

##### 📊 Frequency Calculation

```text
Frequency = f_clk / (ticks × Prescaler)
Frequency = 16,000,000 / (20000 × 8) = 16,000,000 / 160,000 = 100 Hz
```

So the signal has a **period of 10 ms** and a **frequency of 100 Hz**.

> [!TIP]
Use `ICR1` inside the `TIMER1_CAPT_vect` ISR to capture edge timing, and apply these formulas to analyze external signals like PWM, square waves, or sensor pulses.


## Summary Table

| Measurement Type | Formula                                | Notes                          |
|------------------|----------------------------------------|--------------------------------|
| Pulse Width      | `(ΔICR1 × N) / f_clk`                  | Between rising and falling edge|
| Frequency        | `f_clk / (ΔICR1 × N)`                  | Between two rising edges       |
| Period           | Same as pulse width (if full cycle)    | Use two consecutive rising edges|
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

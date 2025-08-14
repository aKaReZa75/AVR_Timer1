This file covers all interrupt-related features of **Timer1**, such as **overflow**, **compare match A/B**, and **input capture** interrupts. It explains how to enable and clear them properly using the **TIMSK1** and **TIFR1** registers.

---

## **TIFR1 (Timer/Counter Interrupt Flag Register)**

| Bit | Name  | Description                             |
|-----|-------|-----------------------------------------|
| 5   | ICF1  | Input Capture Flag                      |
| 2   | OCF1B | Output Compare Match Flag for Channel B |
| 1   | OCF1A | Output Compare Match Flag for Channel A |
| 0   | TOV1  | Timer/Counter Overflow Flag             |

- **OCF1A / OCF1B**:  
  Set when `TCNT1` matches `OCR1A` or `OCR1B`.

- **TOV1**:  
  Set when Timer1 overflows (from `0xFFFF` to `0x0000`). Must be cleared by writing logic **1**.

- **ICF1**:  
  Set when an edge is detected on the ICP1 pin and captured into `ICR1`.

> [!NOTE]  
> Before enabling an interrupt, you should **manually clear the corresponding interrupt flag** in `TIFR1`.  
> Otherwise, if the flag is already set, the ISR will be triggered **immediately after enabling global interrupts**, even if the event hasn't occurred yet.

> [!TIP]  
> These flags must be cleared by writing a logic **1** to them. Writing 0 has no effect.  
> You can efficiently clear interrupt flags in `TIFR1` using the macro: `intFlag_clear(Reg, Bit)`  
> Example:  
> To clear the **Timer1 Overflow Flag** (`TOV1`):  
> `intFlag_clear(TIFR1, TOV1);`

---

## **TIMSK1 (Timer/Counter Interrupt Mask Register)**

| Bit | Name   | Description                               |
|-----|--------|-------------------------------------------|
| 5   | ICIE1  | Input Capture Interrupt Enable            |
| 2   | OCIE1B | Output Compare Match Interrupt Enable (B) |
| 1   | OCIE1A | Output Compare Match Interrupt Enable (A) |
| 0   | TOIE1  | Timer Overflow Interrupt Enable           |

- **ICIE1**: Enable interrupts on input capture events.
- **OCIE1A / OCIE1B**: Enable interrupts on output compare matches.
- **TOIE1**: Enable Timer1 overflow interrupt.

> [!CAUTION]  
> Always ensure that global interrupts are enabled using the `sei()` function.  
> Without enabling global interrupts, the microcontroller will not respond to any interrupt triggers.

You can use the following macros to manage global interrupts:

- `globalInt_Enable` → Enables global interrupts (`sei()`)
- `globalInt_Disable` → Disables global interrupts (`cli()`)

---

### Timer1 Overflow Interrupt

```c
/* ISR for Timer1 Overflow */
ISR(TIMER1_OVF_vect)
{
    /* Code executed on Timer1 overflow */
}

/**< Enable Timer1 Overflow interrupt */
bitSet(TIMSK1, TOIE1);

/* Clear the overflow flag */
intFlag_clear(TIFR1, TOV1);
```

---

### Timer1 Compare Match A Interrupt

```c
/* ISR for Compare Match A */
ISR(TIMER1_COMPA_vect)
{
    /* Code executed when TCNT1 == OCR1A */
}

/**< Enable Compare Match A interrupt */
bitSet(TIMSK1, OCIE1A);

/* Clear compare match A flag */
intFlag_clear(TIFR1, OCF1A);
```

---

### Timer1 Compare Match B Interrupt

```c
/* ISR for Compare Match B */
ISR(TIMER1_COMPB_vect)
{
    /* Code executed when TCNT1 == OCR1B */
}

/**< Enable Compare Match B interrupt */
bitSet(TIMSK1, OCIE1B);

/* Clear compare match B flag */
intFlag_clear(TIFR1, OCF1B);
```

---

### Timer1 Input Capture Interrupt

```c
/* ISR for Input Capture */
ISR(TIMER1_CAPT_vect)
{
    /* Code executed when an edge is captured into ICR1 */
}

/**< Enable Input Capture interrupt */
bitSet(TIMSK1, ICIE1);

/* Clear input capture flag */
intFlag_clear(TIFR1, ICF1);
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

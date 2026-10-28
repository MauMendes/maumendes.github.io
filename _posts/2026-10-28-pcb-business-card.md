---
layout: post
title: "My Open-Source KiCad Business Card"
date: 2026-10-28 12:00:00 +0100
author: mauricio
categories: [hardware]
tags: [pcb, kicad, open source, ai, electronica]
image:
  path: /assets/img/pcb-card/card_back_3d.png
  alt: The back of my PCB business card, full of footprints and reference tables
---

This year I'm going to [**electronica in Munich**](https://electronica.de/). Every time I go to a trade fair, the same idea comes back: *I should make a PCB business card.* This time I really did it! 🎉

And the best part: the **KiCad project is open source**, so you can take it and make your own.

My only rule was: **it must be useful on the bench**. If you keep it next to your soldering station, then it is a good card.

![Front of the card](/assets/img/pcb-card/card_front_3d.png)
_The front: name, contact and a QR code that brings you to this blog._

## What is on the back

All the gold parts are real copper (ENIG), in real size:

- **Pin pitch combs** on the top border: 0.5 mm (FFC), 1.0 (JST SH), 1.25 (JST GH), 1.27, 2.0 (JST PH), 2.5 (JST XH) and 2.54 mm (Dupont). Put a connector on it and you know what you have.
- **Chip resistors and capacitors** from 0201 to 2512.
- **Diodes** (SOD-523 up to SMC) and **tantalum** cases A to D, with the polarity mark.
- **IC packages** from SOT-23 and DFN-8 up to LQFP-48, all with pin 1 marked.
- **Trace width → current** (1 oz copper, +10 °C) and **voltage → clearance** (IPC-2221, outer layer). These are the two things I always look up when I route a board.
- **Mounting holes** M2, M2.5 and M3, and **wire gauge holes** from AWG 28 to 18. Put a wire in the hole and you know the gauge.
- A **cm ruler** on the front and an **inch ruler** on the back.

![Back of the card](/assets/img/pcb-card/card_back_3d.png)

## Specs

| | |
|---|---|
| Size | 85 × 55 mm, 3 mm round corners |
| Thickness | 0.8 mm FR-4 |
| Finish | black solder mask, white silkscreen, ENIG |
| Design | KiCad 10, footprints from the standard KiCad libraries |
| License | CERN-OHL-P-2.0 (permissive open hardware) |

## How it was made

I made more than fifteen versions in about one day. The first ones were only "decoration": gold circuit lines, waveforms, a field of vias... and none of them was really me. Then I remembered a PCB reference ruler that I have on my desk. Full of useful things, a bit nerdy. That was it! After that, the work was mostly to fit more things in: resistors and diodes standing vertical, trace and clearance tables in the same line, and the holes in a place where both sides of the card are free.

## Designed together with AI

I didn't have to open KiCad. I started with a chat with an AI(Claude). I just asked:

```text
I'm going to electronica. can you help me to make a PCB business card. as open source engineer not busnissesman. Show me some ideas. 
```

Then it was a loop: the AI made a quick mock-up, I answered in one line (*"Still didn't get my taste"*, *"More crowded"*, *"Missing pin 1 mark in some ICs"*), and it made the next version. Then I sent a photo of a PCB ruler that I like, and that changed everything.

The AI did the boring work: the real footprint sizes, the IPC-2221 numbers, the QR code, checking that the holes don't hit anything on the other side. At the end it also built the KiCad project, ran the DRC and generated the Gerber files. Then I just manufactured it at JLCPCB (no affiliate link, yet).

**Total cost for 30 cards** (0.8 mm, black, before shipping):

| Finish | Price | Per card |
|---|---|---|
| **ENIG** (gold) | €33.57 | ~€1.12 |
| HASL (silver) | €15.80 | ~€0.53 |

The ENIG option costs €17.77 more, but the gold copper *is* the design, and it doesn't oxidise in your wallet.

What I learned: with words alone I could not explain what I liked. **One picture did it.**

### Inspiration
- 👉 [Hackaday.io – PCB business cards](https://hackaday.io/search?term=pcb%20business%20card): hundreds of ideas, NFC would be too much for me. Maybe revision 2.0
- 👉 [George Hilliard – "My business card runs Linux"](https://www.thirtythreeforty.net/posts/2019/12/my-business-card-runs-linux/): the famous one. Plug it into USB and it boots Linux! - When I grow up I want to do one like these.
- PCB reference rulers: practical, and the reason my card became a tool.

## Get the files and make your own

Everything is on GitHub: 👉 **[github.com/MauMendes/pcb-business-card](https://github.com/MauMendes/pcb-business-card)**

- the KiCad 10 project (no schematic, it is all layout),
- the **gerbers** and the notes I used to order at JLCPCB,
- the Python script that makes the QR code.

To make your own: change the name, e-mail and QR on the front, keep (or remove) what you want on the back, run DRC, export the gerbers and order it like a normal 2-layer board: **0.8 mm, black, ENIG**. One tip: the 0.1 mm clearance example is at the minimum spacing of the manufacturer, so keep it exactly like this or make it 0.13 mm.

Pull requests are welcome! More footprints, more tables, other languages. And if you make one, send me a picture.

If we meet at electronica, ask me for one. 🙂

**Viel Spaß!**

---

_Note: the text of this post was beautified with the help of AI. The ideas, the decisions and the card are mine._

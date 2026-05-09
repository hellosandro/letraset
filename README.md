# letraset
A typeface specimen inspired by old Letraset Catalogs

Type Specimen Collection
A series of single-page A4 type specimens for personal use — printed and hung on the wall as a visual reminder of favourite typefaces and their forms. Inspired by Letraset catalogue specimens.
Each sheet uses the same template: large hero glyph "Ea1", weight scale with subject lines, character set, sample text, header and footer. Output is HTML, designed to be printed via Chrome → ⌘P → Save as PDF.
Current collection
N°FontSourceNotes01Manuale RomanGoogle FontsCosgaya & Tunni, 6 weights 300–80002Manuale ItalicGoogle FontsSame family, italic only03OnestGoogle FontsMartin Vacha, 9 weights 100–900, no italics04Pressio Stencil CompressedAdobe FontsSignal Type Foundry, 5 weights 300–80005InterstateAdobe FontsTobias Frere-Jones, 9 weights 100–90006DiscordiaAdobe FontsÁlvaro Franca / Naipe Foundry, mini-super-family of 4 styles (Regular wedge serif, Italic monolinear sans, Bold slab, Bold Italic display)
Files are named [Font]_Specimen.html. Open `index.html` for a landing page that links to all specimens, or open any specimen file directly in Chrome. To print: ⌘P → margins "None" → A4 → Save as PDF.
Design system
The template is a fixed grid:
┌────────────────────────────────────────────────────────┐
│  Header: font name · descriptor · foundry              │
├────────────────────────────────────────────────────────┤
│                                  Font Name             │
│   E a 1                          (large)               │
│   (giant hero glyph)             ────                  │
│                                  meta block            │
│                                  designer credit       │
├────────────────────────────────────────────────────────┤
│  800 EXTRABOLD  Burj Khalifa, Dubai · 828 m   56 pt   │
│  700 BOLD       Merdeka 118, Kuala Lumpur     46 pt   │
│  ... (one line per weight, sample shrinks proportionally)
├────────────────────────────────────────────────────────┤
│  CHARACTER SET    │  SAMPLE TEXT · 11/15 · REGULAR    │
│  ABCDEFGHIJK...   │  Lorem ipsum...                   │
│  abcdefghijk...   │  ...                              │
├────────────────────────────────────────────────────────┤
│  Source · License    set in [Font] 100—900    N° XX 1 │
└────────────────────────────────────────────────────────┘
Typography conventions

Showcase font (Manuale, Onest, Pressio, Interstate, etc.) is used only where it's being shown: Ea1 hero, font name, weight scale samples, character set, sample text body.
Satoshi is the system font for all "graphic furniture": header, footer, weight labels (e.g. "800 EXTRABOLD"), dimension labels (e.g. "56 pt"), section labels (CHARACTER SET, SAMPLE TEXT), meta block, page number. Satoshi is embedded as base64 inside each HTML file so it works offline.
Specimen number uses N° (U+00B0 degree sign) and not № (U+2116) — the latter is missing from many fonts and from WinAnsi encoding.
Hero glyph "Ea1" is a tribute to original Letraset catalogues. Always 240pt, line-height 0.82, letter-spacing -0.01em.
Hero name (font name top-right) is 18pt — small enough not to interfere with the Ea1 glyph for any font name length, including long ones like "Source Serif Pro".
Specimen number circle in the footer is hand-drawn with border-radius: 50%.

Layout grid (CSS)
The page is a flex column inside a fixed-size container:

A4 (210 × 297mm) with margins MX = 14mm (sides), MY = 14mm (top/bottom)
Hero block: ~80mm tall
Scale block: variable, depending on number of weights
Box block (charset + sample): 1fr (fills remaining)
Footer: ~10mm tall

The hero is a 2-column × 2-row CSS grid:

Row 1: empty / hero name (right-aligned)
Row 2: empty / meta block + credits
Hero glyph spans both rows on the left

This way the font name lives on its own row, never colliding with the Ea1 glyph regardless of length.
Weight scale rules

Each row has 3 columns: weight label (22mm) | sample (1fr) | dimension label (18mm)
Sample is truncated character-by-character if it exceeds the available width — see the JS at the end of each HTML file
padding: 0 0 2.5mm 0 on .row so descenders ('g', 'y', 'j', 'p') don't get cut by the separator line
The sample text scales with the weight: heaviest weight = largest size, lightest weight = smallest size. Subject lines should grow accordingly: short on heavy weights ("Burj Khalifa, Dubai · 828 m"), long and detailed on light weights (multiple entries with metadata).

Subject themes per specimen

01-02 Manuale → world museums in their original language (Musée du Louvre, Galleria degli Uffizi, etc.)
03 Onest → world airports with codes (LHR · London Heathrow)
04 Pressio → skyscrapers with city and height (Burj Khalifa, Dubai · 828 m)
05 Interstate → roads and highways (Route 66, I-95 Miami to Houlton)
06 Discordia → trickster gods and chaos deities (Eris, Loki, Anansi, Coyote, Hermes, etc.)

Each new specimen should pick a subject that resonates with the font's character. Names in non-Latin scripts are written in English transliteration (avoid CJK glyphs since most Latin fonts don't include them).
Sample text rules

ASCII-only, English. No accented characters or special symbols that may not exist in every font.
One paragraph by default, fits in the right box at 11pt/15pt.
Two paragraphs only if there's room (Pressio uses two paragraphs at 11/15 + 15/19 with separating label).
Topic should be related to the row subject (eels for Manuale museums-and-water theme, Wright brothers for airports, skyscraper history for skyscrapers, FHWA history for highways).

Adobe Fonts integration
For Adobe Fonts specimens (Pressio, Interstate, future ones), the HTML loads the kit via:
html<link rel="stylesheet" href="https://use.typekit.net/xtn6ysm.css">
The CSS uses the kit's exact font-family names. To find them, fetch the CSS URL above and inspect the @font-face declarations. The kit currently includes:

franklin-gothic (5 weights)
kensington (5 weights)
pressio-stencil-cmp (5 weights)
interstate-hairline (9 weights with italics — note: despite the name, this is not the Hairline subfamily; it's standard Interstate)
discordia (4 styles: Regular, Italic, Bold, Bold Italic — slug assumed; verify in kit CSS)

To use a different Adobe Fonts kit, replace xtn6ysm with your kit ID in the <link> tag and update the font-family in CSS.
Known issues
Font metrics & Ea1 alignment
Different fonts have different side-bearings and vertical metrics. The default rule (margin-left: 0) works for Manuale and Onest but Interstate's "E" sits slightly to the right of the page margin and the baseline of "Ea1" is too far from the scale's separator line below.
For Interstate specifically:

margin-left: -22pt aligns the E's left edge to the page margin
Vertical baseline alignment is still unsolved — attempted margin-bottom, transform: translateY, and line-height adjustments all caused other layout breakages. See git log.

For future fonts, expect to calibrate Ea1 position. A general solution would be CSS variables --hero-pull-x and --hero-pull-y per specimen, set after a visual check.
Other known issues

Onest doesn't have the dagger († ‡) glyphs — character set was edited to remove them.
The font names "interstate-hairline" in the Adobe kit doesn't actually render Hairline weights; it renders standard Interstate. This is an Adobe kit configuration issue, not the font.

How to add a new specimen

Pick a font and source (Google Fonts → download zip; Adobe Fonts → add to existing kit or create new).
Duplicate the closest existing specimen as starting point:

Roman sans → use Onest_Specimen.html
Roman serif → use Manuale_Specimen.html
Italic-only sheet → use Manuale_Specimen_Italic.html
Adobe Fonts → use Pressio_Specimen.html or Interstate_Specimen.html


Replace:

<link> tag (Google Fonts URL or Adobe Fonts kit URL)
font-family in CSS
<title>
Header text (font name · descriptor · foundry)
Hero name
Meta block (number of weights, range, scripts, designer)
Footer ("set in [Font] X — Y" and Specimen N° XX)
Weight scale <div class="row"> blocks (one per weight)
.r{weight} .sample font-weight rules in CSS
Character set flow (remove glyphs the font doesn't include)
Sample text (related theme to the subject)


Open in Chrome and visually QA. Calibrate Ea1 positioning if needed.
Print: ⌘P → margins "None" → A4 → Save as PDF.

Tech notes

Pure HTML + CSS + minimal vanilla JS (only for the scale row truncation logic at the end of each file).
No build system, no dependencies. Single-file artifacts.
Satoshi font is embedded as base64 inside the <style> block of each HTML — this makes the file ~190KB but ensures offline / no-CDN-dependency.
All measurements use mm and pt to match print-design conventions. Conversion: 1mm ≈ 2.835pt.

To do

 Calibrate Ea1 vertical alignment for Interstate
 Fix the remaining specimens to use the same row padding (descenders not cut) as Onest — currently only Onest had this fix applied; Manuale Roman/Italic and Pressio still have the older padding: 0.6mm 0 which may cut descenders
 Add specimen for an italic-only Onest variant (skip — Onest doesn't have italics)
 Make weight calibration generic via CSS custom properties (--hero-pull-x, --hero-pull-y)
 Add specimens for: Fraunces, Bricolage Grotesque, Instrument Serif, Departure Mono (display, expressive)
 Add Fontshare specimens: Erode, Khand, Tanker, Excon, Gambarino
 ~~Eventually unify all specimens into a single index page with thumbnail navigation~~ → done in `index.html` (typographic list, each entry set in its own typeface)

Conversation history
This project was built iteratively in conversation with Claude (Sonnet/Opus). The full transcript captures every design decision, mistake, and reasoning. If continuing in a new session, share the most recent specimen as reference and the AI should pick up the conventions from the file alone.

For personal use only. Fonts retain their respective licenses (OFL for Google Fonts, Adobe Fonts EULA for Pressio and Interstate).

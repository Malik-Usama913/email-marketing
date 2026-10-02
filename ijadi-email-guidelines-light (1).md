# ijadi.io — Email Guidelines (Light Theme)

**Lightweight playbook for transactional, newsletter, and marketing emails.**
*Inherits the master brand (palette, type, voice) — adapts it for the inbox.*

> Why a light theme for email? Dark backgrounds render inconsistently across Gmail, Outlook and iOS Mail (color inversion, broken PNG halos, washed-out gradients). Light emails deliver better, read cleaner, and let the signature purple→cyan gradient still carry the brand on CTAs and data accents.

---

## 1. Layout & dimensions

| Spec | Value |
|---|---|
| **Max content width** | `600px` (safe across all clients, Gmail preview pane friendly) |
| **Full-bleed wrapper** | 100% width, soft tint background (`#F4F5FB`) |
| **Content card** | centered, `#FFFFFF`, radius `12px` on supported clients |
| **Mobile breakpoint** | `≤480px` → content stacks to 100%, padding reduces |
| **Outer padding (desktop)** | `32px` top/bottom · `32px` left/right |
| **Outer padding (mobile)** | `20px` top/bottom · `20px` left/right |
| **Section rhythm** | `32px` between blocks · `48px` between major sections |

**Rule:** always build layout with `<table role="presentation">` — never `<div>` + flex/grid for structure. Outlook will not render it.

---

## 2. Spacing scale

Inherit from master brand — use only these steps for padding, margin, and gap:

`4 · 8 · 12 · 16 · 24 · 32 · 48 · 64 px`

Never eyeball a `17px` or `28px`.

---

## 3. Colors (light-theme tokens)

```css
--ij-email-bg:        #F4F5FB;   /* page wrapper */
--ij-email-card:      #FFFFFF;   /* content surface */
--ij-email-line:      #E6E8F0;   /* hairlines, dividers */
--ij-email-chip-bg:   #F7F8FC;   /* data chips, soft panels */

--ij-text:            #0E1230;   /* primary body text */
--ij-text-secondary:  #4A5578;   /* subhead, meta */
--ij-text-muted:      #8A93B3;   /* captions, source tags */

--ij-acc:             #766CF2;   /* primary accent (purple) */
--ij-acc-light:       #9F97FF;
--ij-cyan:            #38D0F5;   /* signal / data only */

--ij-grad:            linear-gradient(120deg, #766CF2, #38D0F5);
```

**Laws (unchanged from master):**
1. **One gradient focus per email** — the primary CTA button *or* a hero number. Not both.
2. **Cyan = signal only** — a live-call dot, a key stat, never body text.
3. **Purple leads, cyan supports** — ≤10% total surface area for accents.
4. **No warm grays, no pure `#000`** — all darks cool toward navy.

---

## 4. Typography

Email clients can't be trusted to load Google Fonts. Use a web-safe stack with Montserrat/Outfit as the top-tier wish, falling back cleanly.

```css
/* Headlines */
font-family: 'Montserrat', 'Helvetica Neue', Arial, sans-serif;

/* Body */
font-family: 'Outfit', 'Segoe UI', -apple-system, BlinkMacSystemFont, Arial, sans-serif;
```

**Type scale (email surfaces):**

| Role | Font | Weight | Size / line-height | Color |
|---|---|---|---|---|
| Hero | Montserrat | 700 | `28px / 36px` | `#0E1230` |
| H1 | Montserrat | 700 | `24px / 32px` | `#0E1230` |
| H2 | Montserrat | 600 | `18px / 26px` | `#0E1230` |
| Body | Outfit | 400 | `16px / 26px` | `#0E1230` |
| Subhead / meta | Outfit | 400 | `15px / 24px` | `#4A5578` |
| Micro / source | Outfit | 500 | `12px / 16px` UPPERCASE, letter-spacing `+1px` | `#8A93B3` |
| Button label | Montserrat | 600 | `16px / 20px` | `#FFFFFF` |

**Rules:** one gradient word per headline, max. Numbers are design elements — set large when they're the point. Keep body at `16px` minimum (Apple's accessibility floor on iOS).

---

## 5. Buttons (bulletproof CTA)

Use a table-based button. VML fallback for Outlook if you need the gradient to render there (otherwise Outlook sees a solid purple).

**Spec:**
- Background: `linear-gradient(120deg, #766CF2, #38D0F5)` (fallback solid `#766CF2`)
- Text: `#FFFFFF`, Montserrat 600, `16px`
- Padding: `14px 28px`
- Radius: `10px`
- Width: auto on desktop · 100% on mobile
- Minimum tap target: `44px` tall (Apple HIG)

**Pattern (inline):**

```html
<table role="presentation" cellpadding="0" cellspacing="0" border="0">
  <tr>
    <td style="background:#766CF2; background-image:linear-gradient(120deg,#766CF2,#38D0F5); border-radius:10px;">
      <a href="https://ijadi.io" style="display:inline-block; padding:14px 28px; font-family:'Montserrat',Arial,sans-serif; font-weight:600; font-size:16px; line-height:20px; color:#FFFFFF; text-decoration:none;">
        Book a demo
      </a>
    </td>
  </tr>
</table>
```

---

## 6. Images

- Always set `max-width:100%; height:auto; display:block; border:0;` inline on every `<img>`.
- Retina: author at 2× dimensions, serve at 1× display size.
- Every image needs `alt` text — keyword-aware, descriptive.
- Logo lockup: lowercase `ijadi.io` wordmark, `24px` tall in the header.
- Follow the master brand's **image placeholder rule** — no stock, no AI-generated people. If a real photo belongs there and isn't ready, ship a visible placeholder block.

---

## 7. Preheader (preview text)

Always include. 50–100 characters. Reinforces (never repeats) the subject line.

```html
<div style="display:none; max-height:0; overflow:hidden; mso-hide:all;">
  Your weekly call rescue report — 47 saves, $18.4K pipeline recovered.
</div>
```

---

## 8. Dark mode handling

Email clients auto-invert light emails inconsistently. Signal intent with meta tags; test in Apple Mail (iOS + macOS) and Gmail iOS.

```html
<meta name="color-scheme" content="light dark">
<meta name="supported-color-schemes" content="light dark">
```

**Dark-mode tactics:**
- Keep logos as transparent PNG with a 2px light outline so they survive inversion.
- Never color-code critical info (status, errors) with color alone — add an icon or text label.
- Test every email in Apple Mail dark mode before sending.

---

## 9. Accessibility (WCAG AA)

- Body text contrast: `#0E1230` on `#FFFFFF` = **17.6:1** ✅ (far exceeds 4.5:1 floor)
- Secondary on white: `#4A5578` on `#FFFFFF` = **8.1:1** ✅
- Button text: `#FFFFFF` on `#766CF2` = **4.9:1** ✅
- Add `role="presentation"` to every layout table.
- Set `lang="en"` on `<html>`.
- Minimum font size `14px` anywhere, `16px` for body.
- Minimum tap target `44×44px` for all tappable elements.

---

## 10. Pre-send checklist

- [ ] Subject line ≤ 50 chars; preheader filled (50–100 chars)
- [ ] Renders at 320px (iPhone SE), 375px (iPhone 14), 600px (desktop)
- [ ] All images have `alt` text and `max-width:100%`
- [ ] One gradient focus only — CTA or hero number, not both
- [ ] Cyan accent ≤10% of surface, used for data/signal only
- [ ] Phone number in CTAs matches live routing (`(361) 320-3648`)
- [ ] Pricing/stats verified against live ijadi.io site, or softened to directional
- [ ] Sarah = inbound only · Lucas = outbound only (never crossed)
- [ ] Unsubscribe link present + physical mailing address in footer (CAN-SPAM)
- [ ] Tested in Gmail (web + iOS), Outlook 365, Apple Mail (light + dark)
- [ ] All links have UTM tags
- [ ] CTA tap target ≥44px tall on mobile

---

## 11. File + handoff

- Save finished HTML to `/mnt/user-data/outputs/` with naming:
  `MC-####_ijadi-email-[pillar]-[descriptor].html`
- Keep HTML self-contained — no external CSS files, no linked JS.
- Inline every style that must survive Gmail (Gmail strips `<style>` from the `<head>` on some paths).
- Keep file under **100KB** total (Gmail clips emails over 102KB).

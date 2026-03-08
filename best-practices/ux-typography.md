# UX Typography Best Practices

**When to use:** When creating or editing UI components, landing pages, homepages, marketing pages, or any user-facing text. Apply these rules proactively to ensure readability and accessibility.

**Target audience context:** These guidelines are especially important for apps targeting users 40+, as many use reading glasses and have presbyopia. Prioritize readability over design trends.

---

## Core Rules

### 1. Minimum Font Sizes (2026 Standards)

| Element | Minimum | Recommended | Reasoning |
|---------|---------|-------------|-----------|
| **Body text** | 16px (1rem) | 18px (1.125rem) | WCAG baseline; 18px better for 40+ users |
| **Descriptions, secondary text** | 15px (0.9375rem) | 16-17px (1-1.0625rem) | Below 15px strains eyes with reading glasses |
| **Bullets, list items** | 16px | 16-17px | Same as body; avoid going smaller |
| **Subheadings** | 20px (1.25rem) | 21-24px (1.3-1.5rem) | Clear hierarchy from body |
| **Large UI text** (CTAs, pricing) | 16px | 18-20px (1.125-1.25rem) | Legibility at a glance |

**Rule of thumb:** If you're considering `0.9rem` (14.4px) or smaller for user-facing text, stop. Use `1rem` (16px) minimum, or `1.05-1.125rem` (16.8-18px) for better accessibility.

### 2. Contrast Ratios (WCAG 2.1)

| Text Type | AA (Minimum) | AAA (Preferred for 40+) |
|-----------|--------------|------------------------|
| **Normal text** (<24px) | 4.5:1 | **7:1** |
| **Large text** (24px+ or 18.5px+ bold) | 3:1 | 4.5:1 |

**Common violations to watch for:**
- Light purple text (`#A78BFA`) on light purple gradient - fails contrast
- Medium gray (`#6B7280`) on white - barely passes 4.5:1, not ideal for 40+
- Any text with `opacity < 0.9` on colored backgrounds - often fails

**Fix:** Use darker colors (e.g., `#4B5563` or darker) on light backgrounds. Test with a contrast checker.

### 3. Line Height (Leading)

| Element | Line Height | Why |
|---------|-------------|-----|
| **Body text, descriptions** | 1.6-1.8 | Breathing room for multi-line reading |
| **Headings** | 1.2-1.3 | Tighter for visual impact |
| **Buttons, short labels** | 1.4-1.5 | Balanced vertical centering |
| **Bullets, list items** | 1.5-1.6 | Enough space to scan quickly |

**Rule:** Never use `line-height < 1.5` for text longer than 10 words.

### 4. Font Weight

- **Body text:** `400` (normal) or `500` (medium) for better rendering on light backgrounds
- **Headings:** `600-700` (semi-bold to bold)
- **Emphasis:** Use `600` (semi-bold) instead of `700` for subtle emphasis
- **Avoid:** `300` (light) for body text - too faint for older eyes

### 5. Letter Spacing

- **Body text:** Default (`normal`) or `0.01em` for slight breathing room
- **All caps labels** (e.g., "PURPOSE", "COMMUNITY"): `0.1em` minimum
- **Headings:** `-0.01em` to `-0.02em` for tighter visual cohesion (optional)

---

## Implementation Checklist

When building a new page or component, verify:

- [ ] All body text is 16px+ (prefer 18px)
- [ ] All descriptions/secondary text is 15px+ (prefer 16-17px)
- [ ] Line height is 1.6+ for paragraphs
- [ ] Contrast is 4.5:1+ (test with WebAIM or similar)
- [ ] Text has `font-weight: 500` on light backgrounds for crispness
- [ ] No `opacity < 0.9` on text (use actual color values instead)
- [ ] Text is readable in both light and dark modes (if applicable)

---

## Example: Common Typography Patterns

### Landing Page Cards

```jsx
// ❌ BAD (too small, poor contrast)
const CardText = styled.p`
  font-size: 1rem;           // 16px - minimum, not ideal
  color: #A78BFA;            // Light color - may fail contrast
  line-height: 1.5;          // Tight for multi-line
`;

// ✅ GOOD (readable, accessible)
const CardText = styled.p`
  font-size: 1.125rem;       // 18px - comfortable for 40+
  color: #5B21B6;            // Dark color - passes 7:1 contrast
  line-height: 1.8;          // Breathing room
  font-weight: 500;          // Crisp rendering
`;
```

### Feature Description Text

```jsx
// ❌ BAD
const Description = styled.p`
  font-size: 0.9rem;         // 14.4px - too small
  line-height: 1.5;
`;

// ✅ GOOD
const Description = styled.p`
  font-size: 1.05rem;        // 16.8px - readable
  line-height: 1.6;
  font-weight: 500;
`;
```

---

## Why This Matters

1. **Presbyopia starts at 40:** Nearly everyone 45+ needs reading glasses for small text
2. **Mobile usage is high:** Small screens + small text = unusable for older users
3. **Bounce rates:** Users leave if they have to squint or zoom
4. **Accessibility:** WCAG compliance isn't optional for many organizations

---

## Testing Your Work

1. **Zoom to 125-150%** in browser - does text still fit comfortably?
2. **Test on mobile** - can you read it at arm's length?
3. **Squint test** - if you have to squint, it's too small or low contrast
4. **Show it to a 50+ year old** - if they reach for reading glasses, adjust

---

## References

- WCAG 2.1 (Level AA contrast requirements)
- iOS Human Interface Guidelines (17pt body text recommendation)
- Material Design 3 (16sp baseline)
- Research: 61% of Americans 65-74 use internet regularly; font size directly impacts retention

---

## When to Apply This

- [x] Creating new landing pages or homepages
- [x] Building UI components (cards, modals, forms)
- [x] Designing pricing pages
- [x] Writing marketing copy that will be displayed on screen
- [x] Reviewing PRs that touch user-facing text
- [x] When a user says "the text is too small" or "hard to read"

Apply these rules **proactively**, not reactively. Better to start with readable typography than fix it after launch.

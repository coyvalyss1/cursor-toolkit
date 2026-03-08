# UX/UI Design Principles

This guide provides actionable UX best practices for building intuitive, accessible, and delightful user interfaces.

---

## Core Principles (Nielsen's 10 Usability Heuristics)

### 1. Visibility of System Status
**Always keep users informed about what's happening**

✅ **DO:**
- Show loading states for all async operations (spinners, skeletons, progress bars)
- Display success/error messages after actions
- Highlight active states (selected tabs, current page in navigation)
- Show upload/download progress
- Indicate when data is saving/syncing

❌ **DON'T:**
- Leave users wondering if their action worked
- Use generic "Loading..." without context
- Hide errors or fail silently

**Example:**
```jsx
// ✅ GOOD - Clear feedback
<Button onClick={handleSave} disabled={isSaving}>
  {isSaving ? 'Saving...' : 'Save Changes'}
</Button>

// ❌ BAD - No feedback
<Button onClick={handleSave}>Save</Button>
```

---

### 2. Match Between System and Real World
**Use familiar language and concepts**

✅ **DO:**
- Use everyday language, not technical jargon
- Follow real-world conventions (trash can for delete, folder for collections)
- Use metaphors users understand (shopping cart, inbox, gallery)
- Present information in natural, logical order

❌ **DON'T:**
- Use developer terminology in user-facing text
- Invent new patterns when standard ones exist
- Use ambiguous icons without labels

**Example:**
```jsx
// ✅ GOOD - Clear, familiar language
<Button>Delete Photo</Button>
<Button>Add to Cart</Button>

// ❌ BAD - Technical jargon
<Button>Deallocate Resource</Button>
<Button>Instantiate Purchase Object</Button>
```

---

### 3. User Control and Freedom
**Users need a clear way out**

✅ **DO:**
- Provide undo/redo functionality
- Allow users to cancel long operations
- Make it easy to go back (breadcrumbs, back buttons)
- Support keyboard shortcuts (Esc to close modals)
- Allow users to exit flows without losing progress

❌ **DON'T:**
- Trap users in flows with no escape
- Make destructive actions irreversible without warning
- Force users to complete multi-step processes

**Example:**
```jsx
// ✅ GOOD - Easy escape
<Modal onClose={handleClose}>
  <CloseButton onClick={handleClose}>×</CloseButton>
  {/* Modal content */}
</Modal>

// ✅ GOOD - Undo option
toast.success('Photo deleted', {
  action: { label: 'Undo', onClick: handleUndo }
});
```

---

### 4. Consistency and Standards
**Don't make users wonder if different words/actions mean the same thing**

✅ **DO:**
- Use consistent terminology throughout the app
- Keep button styles/positions consistent
- Follow platform conventions (iOS/Android/Web)
- Use design system components
- Maintain consistent spacing, colors, typography

❌ **DON'T:**
- Call the same thing by different names (e.g., "artwork" vs "creation" vs "image")
- Change button positions between pages
- Mix different design patterns

**Example:**
```jsx
// ✅ GOOD - Consistent primary action placement
<Dialog>
  <DialogActions>
    <Button variant="text">Cancel</Button>
    <Button variant="contained">Confirm</Button>
  </DialogActions>
</Dialog>

// ❌ BAD - Inconsistent button order
<Dialog>
  <DialogActions>
    <Button variant="contained">OK</Button>
    <Button variant="text">Cancel</Button>
  </DialogActions>
</Dialog>
<Dialog>
  <DialogActions>
    <Button variant="text">Cancel</Button>
    <Button variant="contained">Save</Button>
  </DialogActions>
</Dialog>
```

---

### 5. Error Prevention
**Better than good error messages is preventing errors in the first place**

✅ **DO:**
- Validate input as users type (with helpful hints)
- Disable invalid actions (gray out buttons)
- Provide constraints (date pickers instead of text input)
- Confirm destructive actions
- Save drafts automatically
- Provide clear examples/placeholders

❌ **DON'T:**
- Let users submit invalid forms
- Allow destructive actions without confirmation
- Show cryptic validation errors after submission

**Example:**
```jsx
// ✅ GOOD - Prevent errors
<Input
  type="email"
  placeholder="you@example.com"
  error={!isValidEmail(value) && value.length > 0}
  helperText={!isValidEmail(value) ? "Please enter a valid email" : ""}
/>

// ✅ GOOD - Confirm destructive actions
<Button onClick={() => {
  if (confirm('Delete all photos? This cannot be undone.')) {
    handleDeleteAll();
  }
}}>
  Delete All
</Button>
```

---

### 6. Recognition Rather Than Recall
**Minimize memory load**

✅ **DO:**
- Show recently used items
- Provide autocomplete/suggestions
- Display previews (images, thumbnails)
- Keep navigation visible
- Show context (breadcrumbs, page titles)
- Pre-fill forms with known data

❌ **DON'T:**
- Make users remember information from previous screens
- Hide important options in obscure menus
- Use unclear icons without labels

**Example:**
```jsx
// ✅ GOOD - Visual recognition
<RecentArtworks>
  {artworks.map(art => (
    <Thumbnail key={art.id} src={art.url} onClick={() => select(art)} />
  ))}
</RecentArtworks>

// ❌ BAD - Requires recall
<RecentArtworks>
  {artworks.map(art => (
    <div>{art.id}</div> // User must remember what ID 12345 was
  ))}
</RecentArtworks>
```

---

### 7. Flexibility and Efficiency of Use
**Accelerators for expert users**

✅ **DO:**
- Provide keyboard shortcuts
- Allow customization (themes, layouts)
- Support bulk actions
- Remember user preferences
- Offer quick actions/shortcuts
- Enable power user features (advanced search, filters)

❌ **DON'T:**
- Force everyone through the same slow process
- Ignore keyboard navigation
- Make users repeat the same actions

**Example:**
```jsx
// ✅ GOOD - Keyboard shortcuts
useEffect(() => {
  const handleKeyPress = (e) => {
    if (e.metaKey && e.key === 's') {
      e.preventDefault();
      handleSave();
    }
  };
  window.addEventListener('keydown', handleKeyPress);
  return () => window.removeEventListener('keydown', handleKeyPress);
}, []);

// ✅ GOOD - Bulk actions
<Toolbar>
  <Checkbox onChange={selectAll} />
  {selectedCount > 0 && (
    <>
      <Button>Delete {selectedCount} items</Button>
      <Button>Download {selectedCount} items</Button>
    </>
  )}
</Toolbar>
```

---

### 8. Aesthetic and Minimalist Design
**Don't clutter the interface**

✅ **DO:**
- Show only essential information
- Use progressive disclosure (show more on demand)
- Prioritize content over chrome
- Use whitespace effectively
- Keep visual hierarchy clear
- Remove unnecessary elements

❌ **DON'T:**
- Show all options at once
- Use decorative elements that don't serve a purpose
- Compete for attention with multiple CTAs

**Example:**
```jsx
// ✅ GOOD - Progressive disclosure
<Card>
  <CardHeader>
    <Title>Artwork Details</Title>
  </CardHeader>
  <CardContent>
    <BasicInfo /> {/* Show essential info */}
    <Accordion>
      <AccordionSummary>Advanced Options</AccordionSummary>
      <AccordionDetails>
        <AdvancedSettings /> {/* Hide complex options */}
      </AccordionDetails>
    </Accordion>
  </CardContent>
</Card>
```

---

### 9. Help Users Recognize, Diagnose, and Recover from Errors
**Error messages should be helpful**

✅ **DO:**
- Use plain language (no error codes)
- Explain what went wrong
- Suggest how to fix it
- Provide links to help/support
- Show errors near the problem
- Use appropriate severity (error vs warning)

❌ **DON'T:**
- Show technical error messages
- Blame the user
- Use vague messages ("Something went wrong")
- Hide the error or make it hard to find

**Example:**
```jsx
// ✅ GOOD - Helpful error message
<Alert severity="error">
  <AlertTitle>Upload Failed</AlertTitle>
  The file "photo.jpg" is too large (15MB). 
  Please choose a file smaller than 10MB.
  <Button onClick={handleRetry}>Try Again</Button>
</Alert>

// ❌ BAD - Unhelpful error
<Alert severity="error">
  Error: ERR_FILE_TOO_LARGE_413
</Alert>
```

---

### 10. Help and Documentation
**Provide help when needed**

✅ **DO:**
- Provide contextual help (tooltips, info icons)
- Create searchable help docs
- Offer tutorials for complex features
- Show examples and templates
- Provide onboarding for new users
- Make help easy to access

❌ **DON'T:**
- Assume everything is self-explanatory
- Hide documentation
- Make users search externally for help

**Example:**
```jsx
// ✅ GOOD - Contextual help
<FormField>
  <Label>
    API Key
    <Tooltip title="Find your API key in Settings > Developer">
      <InfoIcon />
    </Tooltip>
  </Label>
  <Input />
</FormField>

// ✅ GOOD - Onboarding
{isFirstTime && (
  <Tutorial steps={[
    { target: '#create-button', content: 'Click here to create your first item' },
    { target: '#gallery', content: 'Your creations appear here' }
  ]} />
)}
```

---

## Mobile-First Design Principles

### Touch Targets
- **Minimum size**: 44x44px (iOS) or 48x48dp (Android)
- **Spacing**: At least 8px between interactive elements
- **Thumb zones**: Place primary actions in easy-to-reach areas

```jsx
// ✅ GOOD - Large touch targets
<Button style={{ minHeight: 44, minWidth: 44, padding: '12px 24px' }}>
  Save
</Button>

// ❌ BAD - Too small
<Button style={{ padding: '4px 8px', fontSize: '10px' }}>
  Save
</Button>
```

### Responsive Design
- **Mobile-first**: Design for small screens, enhance for larger
- **Breakpoints**: 320px (mobile), 768px (tablet), 1024px (desktop)
- **Flexible layouts**: Use relative units (%, rem, vh/vw)
- **Test on real devices**: Emulators aren't enough

---

## Accessibility (WCAG 2.1 Guidelines)

### Color and Contrast
- **Minimum contrast**: 4.5:1 for normal text, 3:1 for large text
- **Don't rely on color alone**: Use icons, labels, patterns
- **Test with color blindness simulators**

```jsx
// ✅ GOOD - High contrast, multiple indicators
<Button 
  style={{ 
    background: '#0066CC', 
    color: '#FFFFFF' // 7:1 contrast ratio
  }}
  startIcon={<SaveIcon />} // Icon + text
>
  Save Changes
</Button>
```

### Keyboard Navigation
- **All interactive elements**: Must be keyboard accessible
- **Focus indicators**: Visible focus states
- **Logical tab order**: Follow visual flow
- **Skip links**: Allow skipping repetitive content

```jsx
// ✅ GOOD - Keyboard accessible
<Modal onClose={handleClose}>
  <FocusTrap> {/* Trap focus inside modal */}
    <CloseButton 
      onClick={handleClose}
      onKeyDown={(e) => e.key === 'Escape' && handleClose()}
    >
      Close
    </CloseButton>
  </FocusTrap>
</Modal>
```

### Screen Readers
- **Alt text**: Descriptive text for images
- **ARIA labels**: For icons and complex widgets
- **Semantic HTML**: Use proper heading levels, lists, etc.
- **Announce changes**: Use aria-live for dynamic content

```jsx
// ✅ GOOD - Screen reader friendly
<img 
  src="sunset.jpg" 
  alt="Golden sunset over mountains with purple clouds"
/>

<button aria-label="Delete photo">
  <TrashIcon aria-hidden="true" />
</button>

<div role="status" aria-live="polite">
  {isSaving && "Saving your changes..."}
</div>
```

---

## Performance UX

### Loading States
- **Instant feedback**: Show something within 100ms
- **Skeleton screens**: Better than spinners for content
- **Progressive loading**: Show content as it loads
- **Optimistic updates**: Update UI before server confirms

```jsx
// ✅ GOOD - Skeleton loading
{isLoading ? (
  <SkeletonCard />
) : (
  <Card data={data} />
)}

// ✅ GOOD - Optimistic update
const handleLike = async () => {
  setLiked(true); // Update UI immediately
  setLikeCount(prev => prev + 1);
  
  try {
    await api.like(photoId);
  } catch (error) {
    setLiked(false); // Revert on error
    setLikeCount(prev => prev - 1);
    toast.error('Failed to like photo');
  }
};
```

### Perceived Performance
- **Prioritize above-the-fold**: Load visible content first
- **Lazy load images**: Use intersection observer
- **Prefetch**: Load likely next pages
- **Animate transitions**: Make waits feel shorter

---

## Form Design Best Practices

### Input Fields
- **Clear labels**: Above or beside inputs, not as placeholders
- **Helpful placeholders**: Show examples, not instructions
- **Inline validation**: Validate as users type
- **Smart defaults**: Pre-fill when possible
- **Appropriate input types**: Use email, tel, date, etc.

```jsx
// ✅ GOOD - Well-designed form
<FormField>
  <Label htmlFor="email">Email Address</Label>
  <Input
    id="email"
    type="email"
    placeholder="you@example.com"
    value={email}
    onChange={handleChange}
    error={!!emailError}
    helperText={emailError}
    autoComplete="email"
  />
</FormField>

// ❌ BAD - Poor form design
<Input 
  placeholder="Email Address" // Label as placeholder
  type="text" // Wrong input type
/>
```

### Form Layout
- **Single column**: Easier to scan and complete
- **Logical grouping**: Related fields together
- **Clear progress**: Show steps in multi-page forms
- **Save progress**: Don't lose user data

---

## Microinteractions

### Feedback
- **Button states**: Hover, active, disabled, loading
- **Transitions**: Smooth, purposeful (200-300ms)
- **Haptic feedback**: On mobile for important actions
- **Sound**: Optional, but can enhance experience

```jsx
// ✅ GOOD - Rich interaction feedback
<Button
  onMouseEnter={() => setHovered(true)}
  onMouseLeave={() => setHovered(false)}
  onMouseDown={() => setPressed(true)}
  onMouseUp={() => setPressed(false)}
  style={{
    transform: pressed ? 'scale(0.95)' : hovered ? 'scale(1.05)' : 'scale(1)',
    transition: 'transform 150ms ease-out'
  }}
>
  Click Me
</Button>
```

---

## Empty States

### Make Empty States Useful
- **Explain why it's empty**: "No photos yet"
- **Show what to do next**: "Upload your first photo"
- **Provide action button**: Clear CTA
- **Use illustration**: Make it friendly, not sterile

```jsx
// ✅ GOOD - Helpful empty state
<EmptyState>
  <Illustration src="/empty-gallery.svg" />
  <Title>Your gallery is empty</Title>
  <Description>
    Start creating to fill your gallery
  </Description>
  <Button onClick={goToCreate}>Create Your First Item</Button>
</EmptyState>

// ❌ BAD - Unhelpful empty state
<div>No data</div>
```

---

## Decision-Making Framework

### When Designing a Feature, Ask:

1. **Is it necessary?** (Remove if not)
2. **Is it clear?** (Can users understand it immediately?)
3. **Is it accessible?** (Can everyone use it?)
4. **Is it consistent?** (Does it match existing patterns?)
5. **Is it forgiving?** (Can users undo mistakes?)
6. **Is it fast?** (Does it feel responsive?)
7. **Is it delightful?** (Does it make users smile?)

---

## Common UX Mistakes to Avoid

### ❌ Don't Do This:
1. **Hiding important actions** behind hamburger menus
2. **Using modals for everything** (they interrupt flow)
3. **Requiring registration** before showing value
4. **Auto-playing videos/audio** without user consent
5. **Infinite scrolling without pagination** (can't find things again)
6. **Carousel/sliders** (users rarely click through)
7. **Fake urgency** ("Only 2 left!" when it's not true)
8. **Disabling copy/paste** in forms
9. **Captchas** when not absolutely necessary
10. **Popup spam** (newsletter signups, notifications)

---

## Testing Your UX

### Quick Tests:
1. **5-Second Test**: Can users understand the page in 5 seconds?
2. **First Click Test**: Do users know where to click first?
3. **Grandmother Test**: Could your grandmother use it?
4. **One-Hand Test**: Can it be used with one hand on mobile?
5. **Squint Test**: Is the visual hierarchy clear when squinting?

### User Testing:
- **Test with real users** (not just team members)
- **Watch them use it** (don't explain or help)
- **Ask them to think aloud**
- **Note where they struggle**
- **Iterate based on feedback**

---

## Resources

### Books:
- "Don't Make Me Think" by Steve Krug
- "The Design of Everyday Things" by Don Norman
- "Hooked" by Nir Eyal
- "100 Things Every Designer Needs to Know About People" by Susan Weinschenk

### Websites:
- Nielsen Norman Group (nngroup.com)
- Laws of UX (lawsofux.com)
- Material Design (material.io)
- Human Interface Guidelines (developer.apple.com/design)

### Tools:
- Contrast checker: WebAIM Contrast Checker
- Color blindness simulator: Coblis
- Accessibility: WAVE, axe DevTools
- User testing: UserTesting.com, Maze.co

---

## Quick Reference Checklist

Before shipping any UI:
- [ ] Loading states for all async actions
- [ ] Error states with helpful messages
- [ ] Empty states with clear next steps
- [ ] Keyboard navigation works
- [ ] Focus indicators visible
- [ ] Color contrast meets WCAG AA (4.5:1)
- [ ] Touch targets at least 44x44px
- [ ] Works on mobile (test on real device)
- [ ] Screen reader friendly (test with VoiceOver/TalkBack)
- [ ] Consistent with existing UI patterns
- [ ] Undo/cancel options for destructive actions
- [ ] Success feedback after actions
- [ ] Forms validate inline
- [ ] No jargon in user-facing text
- [ ] Alt text for all images
- [ ] Tested with real users

---

**Remember**: Good UX is invisible. Users should accomplish their goals without thinking about the interface.

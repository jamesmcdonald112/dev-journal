## Before Writing Code
- [ ] What is the single responsibility?
- [ ] What props/data does it need?
- [ ] Should this be multiple components?
- [ ] Where does state live?

## While Writing Code

### Structure
- [ ] Component < 150 lines?
- [ ] Using semantic HTML?
- [ ] TypeScript types defined?
- [ ] Props interface at top?

### Styling (Tailwind)
- [ ] Using theme values (not arbitrary)?
- [ ] Mobile-first (base → md: → lg:)?
- [ ] Consistent spacing (4, 6, 8, 12)?
- [ ] Extracting repeated patterns?

### Accessibility
- [ ] Semantic elements `(<button>, <input>, <label>)`?
- [ ] All images have alt text?
- [ ] Color contrast > 4.5:1?
- [ ] Keyboard navigation works?
- [ ] Focus states visible?
- [ ] ARIA labels where needed?

### Interactivity
- [ ] Hover states defined?
- [ ] Focus states defined?
- [ ] Loading states (if async)?
- [ ] Error states (if can fail)?
- [ ] Disabled states?

### Performance
- [ ] Images optimized?
- [ ] No unnecessary re-renders?
- [ ] useCallback/useMemo if needed?
- [ ] Lazy loading below fold?

## After Writing Code

### Code Quality
- [ ] Clear variable names?
- [ ] No repeated code (DRY)?
- [ ] Comments explain WHY not WHAT?
- [ ] One component = one file?

### Testing (mental)
- [ ] Can tab through with keyboard?
- [ ] Works on mobile (375px)?
- [ ] Works on desktop (1440px)?
- [ ] Error scenarios handled?

### Documentation
- [ ] Props documented (comments/types)?
- [ ] Complex logic explained?
- [ ] Usage examples?

### Git
- [ ] Committed with clear message?
- [ ] Pushed to repo?
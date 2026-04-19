---
name: ui-grammar
description: "Enforce proper grammar and capitalization in UI text. Use this skill whenever the user asks you to audit, fix, or write UI copy — button labels, page titles, navigation items, headers, chips, cards, tooltips, empty states, error messages, or any other user-facing text. Also trigger when the user mentions inconsistent capitalization in their UI, asks how to handle singular vs plural in their interface, wants to ensure count-driven text (\"X items\", \"X users found\") is correct, or asks you to review a component file for grammar issues. If you see UI text in code and it looks wrong, invoke this skill proactively rather than waiting to be asked."
---

# UI Grammar Enforcement

Two rules govern UI text quality: **consistent title case** for interactive and labeling elements, and **programmatic singular/plural agreement** for any text driven by a count.

---

## Rule 1: Title Case for UI Elements

Titles, buttons, headers, navigation items, chips, and card headings should use **title case** — not sentence case, all-caps, or all-lowercase.

### What gets title case

| Element type | Examples |
|---|---|
| Page/section titles | "User Settings", "Recent Activity" |
| Buttons and CTAs | "Save Changes", "Add New Member", "Export to CSV" |
| Navigation items | "My Projects", "Team Settings" |
| Card headings | "Pending Requests", "Quick Actions" |
| Chip/tag labels | "In Progress", "High Priority", "New" |
| Column headers | "Last Modified", "Assigned To" |
| Dialog/modal titles | "Confirm Delete", "Edit Profile" |
| Tooltip content (when it's a label) | "Toggle Dark Mode" |

### Title case rules

Capitalize the first and last word always. Capitalize all other words **except**:
- Articles: *a, an, the*
- Short coordinating conjunctions: *and, but, or, nor, for, yet, so*
- Short prepositions (under 5 letters): *in, on, at, by, to, up, of, as, via*

**Exception: always capitalize the first word of the string**, regardless of its part of speech.

Good: "Add to Cart", "Sort by Date", "Sign In to Continue"
Bad: "Add To Cart", "sort by date", "SIGN IN TO CONTINUE"

### What does NOT use title case

- Body text, descriptions, helper text → sentence case
- Error messages and validation feedback → sentence case ("This field is required.")
- Placeholder text → sentence case ("Enter your email address")
- Toast/notification body text → sentence case

---

## Rule 2: Programmatic Singular/Plural Agreement

Any UI text that reflects a count must use **code logic** to produce the correct grammatical form. Never hardcode only the plural form and hope counts above 1 are the common case. Never just always use the plural and ignore the singular.

### The core pattern

```js
// Bad — always plural regardless of count
`${count} results found`

// Bad — manual ternary scattered everywhere with no consistency
`${count} ${count === 1 ? 'item' : 'items'}`

// Good — a shared utility keeps this consistent and DRY
pluralize(count, 'item')             // → "1 item" / "3 items"
pluralize(count, 'result', 'found')  // → "1 result found" / "3 results found"
```

Write (or recommend) a simple `pluralize` helper rather than scattering inline ternaries. Keep it project-local unless a library is already in use:

```ts
// A minimal helper that covers most UI cases
function pluralize(count: number, singular: string, suffix = ''): string {
  const noun = count === 1 ? singular : `${singular}s`;
  return suffix ? `${count} ${noun} ${suffix}` : `${count} ${noun}`;
}
```

For irregular plurals (person/people, child/children), pass both forms explicitly:

```ts
function pluralizeWith(count: number, singular: string, plural: string, suffix = ''): string {
  const noun = count === 1 ? singular : plural;
  return suffix ? `${count} ${noun} ${suffix}` : `${count} ${noun}`;
}

pluralizeWith(count, 'person', 'people', 'online')
// → "1 person online" / "4 people online"
```

### Verb agreement follows noun agreement

When the subject changes number, so must the verb:

```
// Bad
`${count} user is waiting`   // breaks at count > 1
`${count} users are waiting` // breaks at count === 1

// Good
`${count} ${count === 1 ? 'user is' : 'users are'} waiting`

// Better — extract to a helper or an i18n key
```

Common verb pairs to watch: *is/are*, *was/were*, *has/have*, *does/do*.

### Zero as a special case

Treat zero as plural in English:

```
0 items (not "0 item")
0 results found
No items yet  ← often better UX than "0 items"
```

If the design calls for an empty state message, prefer a friendly "No [noun] yet" / "No [noun] found" over "0 [noun]".

---

## How to audit and fix a file

When asked to review or fix a file:

1. **Scan for UI text strings** — look for JSX text content, string literals assigned to `label`, `title`, `placeholder`, `aria-label`, `children`, button text, and similar props.

2. **Check each string's element type** — is it a button, header, chip, card title, or body text? Apply the right rule.

3. **Fix title case violations** — apply the rules above, preserving brand names, acronyms, and proper nouns (e.g., "OAuth", "GitHub", "API").

4. **Find count-driven strings** — search for template literals or string concatenations that include a variable followed by a noun (`${n} user`, `count + " item"`, etc.). Check whether they handle both singular and plural correctly.

5. **Consolidate plural logic** — if multiple inline ternaries are doing the same thing, introduce or point to a shared helper.

6. **Report a summary** — list what was changed and why, grouped by rule. This helps the developer understand the pattern, not just accept a diff.

---

## Edge cases and judgment calls

- **Brand names and proper nouns**: Never change capitalization of "GitHub", "OAuth", "iOS", "macOS", product names, etc.
- **Acronyms and initialisms**: Keep as-is: "API Key", "CSV Export", "URL Shortener".
- **Short action-only buttons** ("OK", "Yes", "No"): These are conventionally capitalized as-is.
- **All-caps labels for design emphasis**: If the designer intentionally uses CSS `text-transform: uppercase` and the underlying string is title-cased, the underlying string is correct — flag only the source string.
- **i18n/l10n contexts**: If the project uses a translation library (i18next, react-intl, etc.), recommend plural forms use the library's built-in plural rules rather than a hand-rolled helper.

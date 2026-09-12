# HarvestLink style guide

## Design intent

HarvestLink should feel trustworthy, grounded, calm, and commercially capable. The interface combines agricultural warmth with the clarity expected from a B2B trading tool. Visual decisions should support fast comparison of products, prices, offers, status, and reputation.

Avoid decorative complexity that competes with marketplace information. Prefer generous spacing, strong hierarchy, restrained color, and clear state labels.

## Foundations

### Color roles

The current CSS custom properties in `styles.css` are the implementation source of truth. Use semantic roles instead of copying hex values into components.

| Token | Purpose |
| --- | --- |
| `--ink` | Primary text and high-contrast controls |
| `--muted` | Secondary text, metadata, and helper copy |
| `--cream` | Main page background |
| `--paper` | Cards, panels, and modal surfaces |
| `--olive` | Primary brand/action color |
| `--olive-dark` | Hover and high-emphasis olive |
| `--sage` | Soft agricultural labels and backgrounds |
| `--line` | Borders, dividers, and quiet structure |
| `--gold` | Ratings and warm highlights |
| `--danger` | Destructive or invalid states |

State colors must preserve readable text contrast:

- Open/neutral: paper or warm neutral with ink text.
- Success/accepted: pale green with dark green text.
- Warning/closing soon: restrained amber with dark text.
- Declined/inactive: muted neutral, never red unless an actual error occurred.
- Error: pale red surface with dark red text.

Do not communicate a state through color alone. Pair it with a label such as “Supplier selected,” “Accepted,” or “Not selected.”

### Typography

- Display headings use **DM Serif Display** to give the marketplace an established, editorial character.
- Interface text uses **DM Sans** for clarity at compact sizes.
- Page titles should use responsive sizing rather than fixed oversized values.
- Body text should remain comfortable at approximately 15–17 px with a line height near 1.5.
- Labels and metadata may use uppercase and letter spacing, but sentences should not.
- Prices and primary metrics should use bold weight and tabular-looking alignment where values are compared.

### Spacing

Use a 4 px base rhythm. Common increments are 8, 12, 16, 20, 24, 32, and 40 px.

- 8–12 px: icon/label relationships and compact control gaps.
- 16 px: standard component padding and form rhythm.
- 20–24 px: card and panel separation.
- 32–40 px: page sections and major hierarchy.

Avoid adding arbitrary one-off margins when an existing layout container or gap can express the relationship.

### Shape and depth

- Controls use moderate rounding that feels friendly but not playful.
- Cards and panels use larger radii than buttons.
- Borders provide most surface definition.
- Shadows are subtle and normally appear on hover, elevated modals, or active overlays.
- Do not stack strong borders and strong shadows on the same resting component.

## Layout

### Page shell

Use the shared `.shell` container to constrain content and maintain horizontal gutters. Main content belongs inside `.main.shell`.

### Page heading

`.page-heading` pairs the page purpose with its primary action. On narrow screens, allow the action to move below the heading instead of compressing the copy.

### Grids

- `.product-grid` displays marketplace cards and should collapse progressively.
- `.two-col` is used for primary content plus guidance/profile content.
- `.detail-grid` pairs product imagery with transactional information.
- `.stat-grid` presents a small set of role-specific metrics.

Use a grid only when items have a repeatable relationship. A single piece of content should remain a panel or section.

## Shared components

### Buttons

- `.btn-primary`: the one preferred action in a local context, such as “Place a bid” or “Accept offer.”
- `.btn-secondary`: safe alternative, navigation, or cancellation.
- `.linkish`: low-emphasis section action.
- `.btn-small`: compact action inside a dense offer or table-like row.

Button labels use direct verbs. Keep destructive operations visually distinct and require confirmation when the action is difficult to reverse.

Buttons must:

- Use a real `button` element unless they navigate to a URL.
- Declare `type="button"` inside forms unless they submit.
- Have visible focus styling.
- Remain at least approximately 40 px high for primary touch targets.
- Show disabled/loading state for future asynchronous operations.

### Product cards

A product card presents:

1. Product image
2. Certification/claim labels
3. Category
4. Product title
5. Seller
6. Current bid and unit
7. Bid count and available quantity

The entire card navigates to product detail. Avoid nested interactive links inside this card; seller profile navigation belongs on the detail view.

### Product detail

The detail view prioritizes:

1. Identity, category, quantity, and quality description
2. Product rating
3. Clickable seller identity and reputation
4. Base/current price and close time
5. Role-appropriate actions
6. Bid activity
7. Product reviews

Only customers see bid and product-rating actions. Seller and customer names in activity rows link to their profiles.

### Requirement cards

Requirement cards are keyboard-accessible interactive articles. They show category, quantity, budget, delivery location, buyer, offer count, and lifecycle state.

- Open cards use “Open requirement.”
- Closed/selected cards use “Supplier selected.”
- The accepted state receives a green surface/border treatment.
- Buyer profile links inside the card must not also trigger the card modal.

### Supplier offers

Offer rows contain supplier identity, a supply note, proposed price, and the decision action/state.

- The requirement owner sees all offers and an “Accept offer” action while open.
- Sellers see only their own outcome, not competitors' private prices.
- The accepted row uses `.offer-accepted` and an explicit “Accepted” badge.
- Non-selected offers use quiet “Not selected” language.

### Profiles

The profile hero contains account role, business name, location, rating, review count, and the reciprocal rating action when allowed.

Seller profiles show product listings. Customer profiles show supply requirements. Contact information belongs in a separate business-details panel so its future visibility policy can be changed without restructuring the profile.

### Ratings and reviews

- Render five stars with an accessible text label such as “4.8 out of 5 stars.”
- Show the numeric average beside stars where comparison matters.
- Reviews display author, date, text, and rating.
- Use “Rate product” for product quality and “Rate this seller/customer” for partner reputation.
- If the current account already reviewed the subject, the form becomes an update flow.

### Forms

- Every input has a visible label.
- Placeholder text provides an example, not the only instruction.
- Group related fields in `.form-grid`.
- Use `.span-2` for descriptions that benefit from width.
- Helper text explains constraints or privacy, not obvious labels.
- Validation errors should appear next to the field and in a concise summary for future production forms.

### Modals

- Use one modal at a time within `#modal-root`.
- Include a descriptive heading, short context, close button, and clear footer actions.
- Clicking the backdrop may close a non-destructive modal.
- Escape should close the modal in a future accessibility enhancement.
- Focus should move into the modal and return to its trigger when it closes.
- Decision modals should not close silently while an asynchronous request is pending.

### Toasts

Toasts confirm the result of an action. Use a short result title and one helpful detail. Do not rely on a toast as the only place an important error or persistent status is shown.

## Interaction states

Every interactive component should define:

- Resting
- Hover
- Keyboard focus
- Active/pressed where useful
- Disabled
- Loading for asynchronous production behavior
- Success or accepted
- Validation/error
- Empty state

Transitions should be quick and restrained, generally 120–220 ms. Avoid motion that changes layout unexpectedly.

## Responsive behavior

Primary breakpoints are determined by content rather than specific devices.

- Product and profile grids collapse before card content becomes cramped.
- Two-column layouts become one column on narrow viewports.
- Offer rows stack vertically below approximately 720 px.
- Profile rating controls left-align below the profile identity on mobile.
- Navigation remains reachable through the mobile menu.
- Modal width respects viewport gutters and content remains scrollable.
- Images maintain their aspect ratio and do not force horizontal overflow.

Test at minimum around 360, 768, 1024, and 1440 px widths.

## Accessibility

- Maintain at least WCAG AA contrast for text and controls.
- Use semantic links for navigation and buttons for actions.
- Preserve a logical heading hierarchy.
- Supply meaningful alternative text for product imagery.
- Ensure all card and modal actions work with keyboard input.
- Provide accessible names for icon-only controls.
- Do not remove focus outlines without a visible replacement.
- Announce toast and validation updates through appropriate live regions in a production pass.
- Trap focus within open modals and restore it on close.
- Respect `prefers-reduced-motion` for smooth scrolling and transitions.

## Content style

Voice is practical, respectful, and reassuring.

- Prefer “Place a bid” over “Submit.”
- Prefer “Supplier selected” over ambiguous “Completed.”
- State units beside every price.
- Distinguish “bid” for product auctions from “offer” for supply requirements.
- Use “customer” consistently in the interface; “buyer” is acceptable in explanatory or relationship text.
- Do not promise certification verification unless it has actually occurred.
- Keep empty states actionable and specific.

## Adding a new style

Before adding CSS:

1. Check whether an existing token, layout, or component covers the need.
2. Add a semantic class rather than styling by DOM depth.
3. Define hover and focus behavior for interactive elements.
4. Test the accepted/error/empty state if applicable.
5. Test narrow and wide layouts.
6. Update this guide when the addition creates a reusable rule.


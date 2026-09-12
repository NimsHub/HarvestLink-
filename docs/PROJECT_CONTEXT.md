# HarvestLink project context

## Product summary

HarvestLink is an interactive B2B marketplace prototype for agricultural producers, business buyers, and delivery partners in Sri Lanka. It demonstrates product discovery, competitive product bidding, demand-led sourcing through supply requirements, reciprocal reputation, and role-specific workspaces.

The prototype is intentionally static and dependency-light. It validates workflows and interface decisions before a production backend, authentication service, payment flow, or logistics integration is selected.

## Product goals

- Help agricultural producers publish stock and reach business buyers directly.
- Give customers transparent product bidding and a way to publish demand.
- Let sellers respond privately to customer supply requirements.
- Help customers select a supplier using price, supply notes, profiles, and ratings.
- Establish trust through product reviews and reciprocal seller/customer reviews.
- Provide delivery accounts with visibility into future logistics opportunities.
- Keep the prototype easy to run, review, and deploy.

## Account roles

### Seller

- Publishes agricultural product listings.
- Reviews bids received on listings.
- Discovers customer supply needs.
- Creates or updates private supplier offers.
- Sees whether an offer was accepted.
- Opens customer profiles and rates customers.

### Customer

- Searches and filters products by text, category, and seller.
- Opens product details and places bids.
- Rates products and updates an existing product review.
- Creates supply requirements.
- Compares supplier offers and accepts one.
- Opens seller profiles and rates sellers.

### Delivery

- Views the delivery board and upcoming stock movement opportunities.
- Represents a future logistics workflow; assignment and fulfilment are not yet persisted.

## Implemented workflows

| Workflow | Status | Notes |
| --- | --- | --- |
| Sample account switching | Implemented | Available from the account avatar |
| Account creation by email or phone | Prototype | Creates a local browser account without verification |
| Product publishing | Implemented | Seller-only modal |
| Product search and filtering | Implemented | Text, category, and seller |
| Product detail navigation | Implemented | Hash route |
| Product bidding | Implemented | Customer-only; validates next bid |
| Product reviews | Implemented | Customer-only; supports update |
| Supply requirement creation | Implemented | Customer-only modal |
| Supplier offers | Implemented | Seller-only; an active offer can be updated |
| Supplier selection | Implemented | Requirement owner accepts one offer |
| Seller profiles | Implemented | Products, contact details, rating, and reviews |
| Customer profiles | Implemented | Supply needs, contact details, rating, and reviews |
| Reciprocal account reviews | Implemented | Customer → seller and seller → customer |
| Delivery assignment | Concept only | Board uses representative sample information |
| Real authentication | Not implemented | Requires backend and verification provider |
| Payments and contracts | Not implemented | Outside prototype scope |

## Current architecture

```text
index.html
  ├── styles.css      visual system and responsive components
  └── app.js          seed data, state, rendering, routing, and interactions

Browser
  ├── location.hash   page navigation
  └── localStorage    prototype persistence

GitHub
  └── Actions → Pages static deployment
```

There is no bundler, framework, build step, API, or server-side database. `server.js` is a small local static server used by `npm start`.

## Source map

| Path | Responsibility |
| --- | --- |
| `index.html` | Application shell, font resources, and script/style entry points |
| `styles.css` | Design tokens, layout, components, states, and responsive rules |
| `app.js` | Seed entities, local persistence, role logic, views, modals, and routing |
| `server.js` | Local static file server |
| `package.json` | Local start command and project metadata |
| `.github/workflows/pages.yml` | GitHub Pages deployment |
| `docs/DATA_MODEL.md` | Future persistent relational model |
| `docs/PROJECT_CONTEXT.md` | Product and architecture context |
| `docs/STYLE_GUIDE.md` | UI and content conventions |
| `docs/COMMON_PATTERNS.md` | Reusable implementation patterns |

## Runtime and persistence

- Supported local runtime: Node.js 18 or later.
- Start command: `npm start`.
- Default local URL: `http://127.0.0.1:4173`.
- Production prototype: `https://nimshub.github.io/HarvestLink-/`.
- Storage key: `harvestlink-prototype-v1`.
- Default account: Maya Trading House, a customer.
- Stored prototype data is browser- and origin-specific.
- The app performs small compatibility migrations when newly introduced fields are absent.

Changing the seed does not replace data already stored in a browser. During development, clear the HarvestLink storage key or test with a new browser context when fresh seed data is required.

## Routes

| Hash route | View |
| --- | --- |
| `#home` | Landing/dashboard overview |
| `#market` | Searchable product marketplace |
| `#product/:productId` | Product detail, bidding, and product reviews |
| `#requirements` | Customer requirements or seller demand board |
| `#dashboard` | Role-specific activity/listings/delivery board |
| `#profile/:accountId` | Seller, customer, or delivery profile |

Routes must remain shareable and survive page refreshes on GitHub Pages. Use hash routing until the project adopts hosting with rewrite support.

## Important product rules

- Only customers place product bids and product reviews.
- Only sellers publish products and submit supplier offers.
- Only the customer who created a requirement may accept its offer.
- Accepting one supplier offer closes the requirement and declines competing offers.
- Customers may review sellers; sellers may review customers.
- An account cannot review itself.
- Ratings are displayed to one decimal place.
- User-generated text is escaped before insertion into rendered HTML.
- Role-based controls improve the interface but do not replace backend authorization.

## Current technical decisions

### Static, framework-free prototype

The implementation uses plain HTML, CSS, and JavaScript to minimize setup and make the prototype portable. When application complexity or team size grows, the UI can be migrated incrementally to a component framework.

### Browser persistence

`localStorage` is appropriate only for interactive demonstration. It has no cross-device consistency, authorization, transactions, backups, or concurrency control. The target persistent model is documented in [Data model](DATA_MODEL.md).

### Hash navigation

Hash routes work on a static host without server rewrites. A production application can move to history-based routing after its hosting layer supports fallback routing.

### Embedded prototype collections

Product bids and supplier offers are embedded in parent objects for simple rendering. They become separate tables and API resources in the persistent implementation.

### Progressive data migration

New optional prototype fields are added through compatibility logic rather than resetting stored user activity. Persistent database changes must instead use versioned, reversible migrations.

## Known limitations

- Authentication is simulated; email and phone are not verified.
- Every browser has an independent copy of the data.
- Prices are modeled as whole LKR values and do not yet include tax or fees.
- Product quantities are primarily presentation strings.
- There is no auction close scheduler or server-authoritative clock.
- Reviews are not yet restricted to completed trades in the prototype.
- Contact details are publicly visible within profiles.
- Images are remote sample URLs and have no upload or moderation workflow.
- Delivery assignments, notifications, payments, disputes, and audit logs are not implemented.
- Accessibility is considered in structure and focus behavior but needs a formal audit.

## Production migration boundaries

Introduce a backend behind a repository/service interface instead of rewriting view logic around raw HTTP calls. Recommended boundaries:

- Authentication/session service
- Account/profile service
- Product catalog service
- Product bidding service with atomic bid placement
- Supply requirement and supplier-offer service
- Review service with trade eligibility
- Trade/order service
- Delivery assignment service
- Notification service
- Media upload and moderation service

The server must own validation, authorization, accepted-offer transitions, bid concurrency, review eligibility, and aggregate ratings.

## Quality checklist

Before considering a feature complete:

1. Test it with every affected role and verify forbidden roles do not receive the control.
2. Test empty, populated, updated, and accepted/closed states where applicable.
3. Refresh the page and verify state persists.
4. Navigate directly to the affected hash route.
5. Check desktop and narrow/mobile layouts.
6. Check keyboard access for cards, buttons, modals, and close actions.
7. Check browser console errors.
8. Run `node --check app.js`.
9. Confirm the GitHub Pages workflow succeeds.
10. Update the relevant document in this directory.

## Near-term roadmap

1. Extract a formal data-access adapter from `app.js`.
2. Introduce persistent accounts and verified email/phone authentication.
3. Normalize products, bids, requirements, offers, certifications, and reviews.
4. Create trade records from accepted bids and offers.
5. Restrict reviews to eligible completed trades.
6. Add delivery assignment and status tracking.
7. Add notification and audit-event streams.
8. Add automated unit, integration, and end-to-end tests.


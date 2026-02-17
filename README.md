# Private Marketplace Prototype

A single-page, low-fidelity prototype demonstrating a complete end-to-end mortgage marketplace flow across 4 role-based surfaces with shared demo state.

## Overview

This is a proof-of-concept prototype built to demonstrate stakeholder value across different user roles:

- **Agency Admin**: Create agency accounts and generate agent invite links
- **Agent Sign Up**: Agent onboarding via invite link
- **Agent Dashboard**: Create personalized buyer links and track lead status
- **Private Marketplace**: Buyer experience for viewing mortgage offers and requesting quotes

## Key Features

- **Grayscale Wireframe Design**: Black/white/gray only, no brand colors, minimal styling
- **Global Prototype Controls**: Tab navigation, per-tab Prefill, Reset, and All Reset for easy demoing
- **Cross-Tab State Sync**: When buyer submits quote request, agent dashboard updates status in real-time (Pending → Completed)
- **Per-Tab Prefill**: Each tab has its own demo data - Agency Admin (Horizon Realty), Agent Sign Up (Daniel Reyes), Agent Dashboard (3 sample leads), Private Marketplace (auto-fills on first visit)
- **Two-Tier Offers System**: 3 primary offers always shown, 4 additional offers with "Show More Offers" toggle
- **Copy Animations**: All copy buttons show "Copied" feedback for 2 seconds with Check icon
- **Currency Formatting**: Smart $ formatting for Property Value and Loan Amount with blur/focus handling
- **Relative Timestamps**: Dynamic time display ("sent 2 hours ago", "sent 11 days ago") for realistic demo
- **Auto-Entry Modal**: Private Marketplace shows welcome modal automatically on first visit
- **Zero Backend**: All state managed in-memory with Zustand (1182-line store)

## Tech Stack

- **Vite**: Fast build tool and dev server
- **React 18**: UI library
- **TypeScript**: Type safety
- **Zustand**: Global state management
- **Tailwind CSS**: Utility-first styling
- **Lucide React**: Icon library

## Getting Started

### Prerequisites

- Node.js 18+ installed
- npm or yarn

### Installation

```bash
# Install dependencies
npm install

# Start development server
npm run dev
```

The app will open at `http://localhost:5173`

### Building for Production

```bash
npm run build
npm run preview
```

## Demo Flow

### Quick Start with Prefill

The **Prefill** button populates the current tab with demo data:

- **Agency Admin**: Fills Horizon Realty Group + Sarah Mitchell
- **Agent Sign Up**: Fills Daniel Reyes profile
- **Agent Dashboard**: Creates 3 sample leads (Stacy Barnes, Lewis Hamilton, Michael Tran)
- **Private Marketplace**: Auto-fills James Walker data on first visit

### Complete End-to-End Flow

1. **Agency Admin Tab**
   - Click **Prefill** to fill Horizon Realty Group data
   - Submit to create agency account
   - Copy the agent invite link (shows "Copied" for 2 seconds)

2. **Agent Sign Up Tab**
   - Click **Prefill** to fill Daniel Reyes data
   - Create profile
   - Click "Go to Dashboard" to navigate to Agent Dashboard

3. **Agent Dashboard Tab**
   - Click **Prefill** to create 3 sample leads OR:
   - Click **+ Create** to open Create Link modal
   - Click **Prefill** inside modal to populate James Walker data (note: credit score is blank)
   - Click **Generate Link** (1.5s loading)
   - Copy the link (shows Check icon for 2 seconds)
   - Notice James appears in table with "Pending" status badge
   - Try clicking a lead row to see Lead Detail modal
   - Try clicking Copy icon or Regenerate icon in Actions column

4. **Private Marketplace Tab**
   - Entry modal appears automatically with "Welcome James"
   - All prefilled data is shown as readonly fields (2-column grid)
   - Enter credit score (required field that was blank in agent's snapshot)
   - Click **Find Offers** (3s loading state: "Finding your offers…")
   - View 3 primary offers in the rate table
   - Click **Show More Offers** to see 4 additional offers (7 total)
   - Filters are locked by default - click **Edit** to unlock
   - Try editing a filter value and clicking **Update Offers** (3s loading, then re-locks)
   - Click **Get Quote** on any offer
   
   **Get Quote Modal - Step 1:**
   - Lender is pre-selected (checkbox checked)
   - Try "Select all" / "Deselect all" link
   - Try "Show More Offers" toggle within modal
   - Select 1-3 lenders and click **Continue**
   
   **Get Quote Modal - Step 2:**
   - Name and Email are readonly (James Walker, james.walker@email.com)
   - Click **Prefill** to fill phone number: (704) 555-0199
   - Disclosure text is displayed (no checkbox, just informational)
   - Click **Agree & Submit** (1.5s loading)
   
   **Get Quote Modal - Step 3:**
   - See confirmation: "Your request has been sent."
   - Message: "A confirmation email has been sent to james.walker@email.com"
   - Click **Close**

5. **Verify Cross-Tab Update** (CRITICAL FEATURE)
   - Switch back to **Agent Dashboard** tab
   - Notice James Walker's status badge changed from "Pending" to "Completed" (black filled badge)
   - This demonstrates real-time cross-tab state synchronization

6. **Reset Options**
   - Click **Reset** to reset only the current tab
   - Click **All Reset** to reset entire application to initial state

## Acceptance Criteria Checklist

- ✅ Per-tab Prefill populates appropriate demo data for each view
- ✅ Agent Dashboard Prefill creates 3 sample leads with different statuses and timestamps
- ✅ Create Link modal Prefill fills James Walker data with blank credit score
- ✅ Private Marketplace entry modal blurs background and greets "Welcome James"
- ✅ Entry modal shows all prefilled data as readonly and requires credit score input
- ✅ After "Find Offers", 3-second loading ("Finding your offers…") then 3 offers appear
- ✅ Filters lock by default with Edit button shown
- ✅ Edit unlocks filters and shows Update Offers button
- ✅ Update Offers shows 3-second loading ("Updating offers…") and re-locks filters
- ✅ Show More Offers toggles between 3 and 7 offers in both main view and Get Quote modal
- ✅ Get Quote Step 1: Multi-select with checkboxes, "Select all" link, Continue button
- ✅ Get Quote Step 2: Readonly name/email, required phone, Prefill button, disclosure text
- ✅ Get Quote Step 3: Confirmation with email message "A confirmation email has been sent to james.walker@email.com"
- ✅ After "Agree & Submit", Agent Dashboard shows James status as Completed (CRITICAL cross-tab update)
- ✅ Reset button resets current tab only
- ✅ All Reset button returns entire application to initial state
- ✅ All Copy buttons show "Copied" with Check icon for 2 seconds
- ✅ Lead table Copy and Regenerate icons work with proper animations
- ✅ Relative timestamps display correctly ("sent 2 hours ago", "sent 11 days ago")
- ✅ Currency inputs format on blur with $ symbol and commas
- ✅ Zip code displays city/state (e.g., "New York, NY" for 10011)
- ✅ All icons render from Lucide React (Copy, Check, RefreshCw, DollarSign, Percent, MapPin, ChevronDown, Info)
- ✅ Status badges render correctly (Pending = outlined, Completed = black filled)
- ✅ All modals close properly and maintain state

## Component Features

### Private Marketplace View

**Filters Panel (Left 30%):**
- All mortgage criteria with appropriate input types
- Zip code with city/state display and MapPin icon
- Currency inputs with $ icon and auto-formatting
- Dropdowns with ChevronDown icons using shared `mortgageOptions` constants
- DTI Ratio segmented control (two buttons)
- Info icons next to complex fields (Military Status, DTI Ratio, Mortgage Points, Include FHA)
- Lock/unlock pattern with Edit and Update Offers buttons
- Disabled styling for locked state

**Rate Table (Right 70%):**
- 3 primary offers always visible
- 4 additional offers revealed with "Show More Offers" toggle
- Each offer shows: Lender name, NMLS number, Rate, APR, Monthly Payment, Loan Type, Closing Costs
- Get Quote button per offer
- Loading states: "Finding your offers…" (3s), "Updating offers…" (3s)
- Empty state message when no offers

**Entry Modal:**
- Auto-opens on first Private Marketplace visit
- "Welcome James" headline with Bankrate branding
- Prefilled readonly fields displayed in 2-column grid
- Credit score dropdown (required, initially empty)
- Find Offers button triggers 3s loading

**Get Quote Modal (3 Steps):**
- Step 1: Lender selection with multi-select checkboxes, Select all/Deselect all, Show More toggle
- Step 2: Contact form with Prefill button, readonly name/email, required phone, disclosure text
- Step 3: Confirmation message with buyer email

## Project Structure

```
private-marketplace/
├── src/
│   ├── store/
│   │   └── usePrototypeStore.ts       # Zustand global state (1182 lines)
│   ├── components/
│   │   ├── PrototypeControlBar.tsx    # Top navigation with tabs + controls
│   │   ├── views/
│   │   │   ├── AgencyAdminView.tsx    # Agency account creation
│   │   │   ├── AgentSignUpView.tsx    # Agent profile creation
│   │   │   ├── AgentDashboardView.tsx # Lead tracking table
│   │   │   └── PrivateMarketplaceView.tsx # 30/70 filters + offers
│   │   └── modals/
│   │       ├── CreateLinkModal.tsx    # Personalized link creation
│   │       ├── RegenerateModal.tsx    # Link regeneration
│   │       ├── LeadDetailModal.tsx    # Lead snapshot view
│   │       ├── EntryModal.tsx         # Welcome + credit score
│   │       └── GetQuoteModal.tsx      # 3-step quote request
│   ├── constants/
│   │   └── mortgageOptions.ts         # Shared dropdown options
│   ├── utils/
│   │   └── currency.ts                # Currency formatting helpers
│   ├── App.tsx                        # Main app with tab routing
│   ├── main.tsx                       # React entry point
│   └── index.css                      # Tailwind + custom styles
├── package.json                       # Dependencies
├── tailwind.config.js                 # Custom grayscale palette
├── vite.config.ts                     # Vite configuration
├── tsconfig.json                      # TypeScript config
├── README.md                          # This file
├── TESTING.md                         # Detailed test checklist
├── QUICKSTART.md                      # 5-minute demo guide
└── PROJECT_SUMMARY.md                 # Implementation summary
```

**Total Components:** 11 (1 control bar + 4 views + 6 modals)  
**Total Source Lines:** ~2,500+  
**Dependencies:** 183 packages

## Sample Data

### James Walker (Primary Demo Identity)

Used in Create Link modal Prefill and Private Marketplace:

- Name: James Walker
- Email: james.walker@email.com
- Zip: 10011 (New York, NY)
- Property Value: $850,000
- Loan Amount: $680,000
- Down Payment: 20%
- Credit Score: **BLANK** (filled in Entry Modal)
- Loan Term: 30 year fixed
- Military Status: Non-Military
- DTI Ratio: Less than 40%
- Mortgage Points: All
- Property Type: Single Family
- Property Use: Primary
- Include FHA: No

### Prefilled Leads (Agent Dashboard)

1. **Stacy Barnes** - Pending - 2 hours ago - Full form snapshot
2. **Lewis Hamilton** - Pending - 11 days ago - Minimal snapshot (email only)
3. **Michael Tran** - Completed - 3 days ago - Minimal snapshot (email only)

### Mortgage Offers

**Primary Offers (always shown):**
1. First National Bank - NMLS #3304 - 6.875% rate - $4,478/mo
2. Metro Mortgage Corp - NMLS #2156 - 6.950% rate - $4,526/mo
3. Coastal Credit Union - NMLS #1789 - 7.000% rate - $4,559/mo

**Additional Offers (Show More):**
4. Horizon Home Lending - NMLS #4521 - 7.050% rate - $4,592/mo
5. Liberty Financial Group - NMLS #5892 - 7.100% rate - $4,625/mo
6. Summit Lending Partners - NMLS #6734 - 7.125% rate - $4,641/mo
7. Nationwide Trust Bank - NMLS #7208 - 7.150% rate - $4,658/mo

## Timing Specifications

All loading states and animations use exact timings:

- **Copy Animations:** 2000ms (2 seconds) - All copy buttons show "Copied" feedback
- **Generate Link:** 1500ms (1.5 seconds) - Create Link modal loading
- **Regenerate Link:** 1000ms (1 second) - Regenerate modal loading
- **Find Offers:** 3000ms (3 seconds) - Entry modal → offers
- **Update Offers:** 3000ms (3 seconds) - Filter changes → refreshed offers
- **Submit Quote:** 1500ms (1.5 seconds) - Quote request submission

## Design Decisions

### Why Separate Surfaces?

Each view represents a different role and moment in the buyer journey, demonstrating how the system creates value across stakeholders (agency admin, agent, buyer).

### Why Per-Tab Prefill?

Different tabs need different demo data. Per-tab prefill allows demoing individual sections without affecting other views, providing more flexibility than global prefill.

### Why Global Controls?

Prototype controls (tabs, Prefill, Reset, All Reset) are separated from product UI to enable rapid demoing without manual typing. Stakeholders can quickly see the complete flow and reset for multiple demo runs.

### Why Lock Filters After Submission?

This demonstrates "serious buyer" behavior - commit to criteria, get offers, then decide. It encourages thoughtful requests vs. endless exploration. The Edit button allows adjustments while maintaining the "locked-in" mental model.

### Why Two-Tier Offers (3 + 4)?

Shows 3 primary offers by default to avoid overwhelming the UI, while "Show More Offers" reveals 4 additional lenders. This demonstrates scalability without cluttering the initial view.

### Why Cross-Tab Status Update?

**This is the key value demonstration:** When a buyer submits a quote request in Private Marketplace, the agent immediately sees the status change from Pending to Completed in Agent Dashboard, showing real-time system integration across user roles.

### Why Currency Formatting?

Property Value and Loan Amount use smart formatting (`$850,000`) that formats on blur but allows free typing. This balances UX polish with prototype simplicity.

### Why Relative Timestamps?

Dynamic time formatting ("sent 2 hours ago", "sent 11 days ago") makes the demo feel realistic with dated sample data, showing how the system would look in actual use.

## Styling & Icons

### Custom Tailwind Colors

Defined in `tailwind.config.js`:

- `wire-white`: #FFFFFF
- `wire-black`: #000000
- `wire-gray-light`: #E5E5E5
- `wire-gray`: #999999
- `wire-gray-dark`: #666666

### Component CSS Classes

Defined in `src/index.css`:

- `.btn-primary` - Black fill, white text, hover gray-dark, disabled gray
- `.btn-secondary` - White fill, black border/text, hover gray-light
- `.input-field` - White bg, black border, focus ring-2, disabled gray
- `.modal-overlay` - Fixed overlay with 50% black opacity
- `.modal-content` - White bg, 2px black border, max-height 90vh
- `.badge-pending` - Outlined border-only style
- `.badge-completed` - Black filled with white text
- Custom checkbox styling with black fill and white checkmark

### Icons from Lucide React

- **Copy** - Copy link buttons (changes to Check when copied)
- **Check** - Copy confirmation state
- **RefreshCw** - Regenerate link button
- **DollarSign** - Currency input visual indicator
- **Percent** - Down payment input visual indicator
- **MapPin** - Zip code location display
- **ChevronDown** - Dropdown menu indicator
- **Info** - Field help/tooltip icons

## Technical Notes

- This is a **prototype**, not production code
- **No authentication** - no login, sessions, or auth flows
- **No validation** - forms accept any input without checks
- **No error handling** - assumes happy path for demo purposes
- **In-memory state** - all data in Zustand store, resets on page refresh
- **Simulated timestamps** - relative time calculated from Date objects
- **No backend** - no API calls, database, or server
- **No routing** - conditional rendering based on `currentTab` state
- **Zero persistence** - state cleared on refresh or All Reset

## Architecture Highlights

### State Management

Single Zustand store (`usePrototypeStore.ts`) with:
- 1182 lines of state, actions, and logic
- All 4 views share the same store
- Cross-tab update via shared state: `updateLeadStatus(email, status)`
- Helper functions: `generateFakeOffers()`, `generateAdditionalOffers()`

### Shared Utilities

- **constants/mortgageOptions.ts** - Dropdown options used in Create Link modal and Private Marketplace filters (credit scores, loan terms, property types, etc.)
- **utils/currency.ts** - Currency formatting functions: `formatCurrency()`, `handleCurrencyChange()`, `handleCurrencyBlur()`

### Component Organization

- **1 Control Bar** - Global navigation and demo controls
- **4 Views** - One per user role/surface
- **6 Modals** - Focused flows without page navigation
- **Conditional rendering** - All views in single page, switched via `currentTab`

## License

Internal prototype - not for distribution

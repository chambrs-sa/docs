# 🧩 CHAMBRS Non-Functional Meta Prompts

## **Meta Prompt #1: Update Color Scheme to Black-Purple Gradient**

**Objective:**  
Replace the current blue-based color palette with a black (`#000000`) to purple (`#3432c7`) linear gradient theme.

### **Instructions**
1. **Update `src/index.css`:**
   - Replace the current `--gradient-primary` with  
     `linear-gradient(135deg, #000000 0%, #3432c7 100%)`
   - Update `--gradient-accent` to the same value.
   - Add a new variable `--gradient-brand`.
   - Update `--primary` color: `265 100% 50%` (HSL of `#3432c7`)
   - Update `--accent`: `265 80% 60%`

2. **Update `tailwind.config.ts`:**
   - Add `"gradient-brand": "var(--gradient-brand)"` in `backgroundImage`.
   - Ensure Tailwind exposes it as a utility class.

3. **Verify:**  
   All buttons, cards, and UI elements reflect the new theme.

**✅ Expected Outcome:**  
Full app adopts a black→purple gradient palette with consistent primaries.

---

## **Meta Prompt #2: Create Sticky Global Header Component**

**Objective:**  
Implement a persistent header (like EasyEquities) that stays on top across all pages.

### **Instructions**
1. **Create `src/components/Header.tsx`:**
   - `sticky top-0 z-50`
   - Background: `gradient-brand`
   - CHAMBRS logo/text left (clickable)
   - Nav links right: `Invest | Download App | About | Support`
   - Mobile hamburger + shadow on scroll

2. **Navigation logic:**
   - *Invest* → `/assets` (auth) or `/auth`
   - *Download App*, *About*, *Support* → placeholders (`#`)

3. **Conditional rendering:**
   - Landing → show “Get Started” / “Sign In”
   - Authenticated → user avatar + sign-out

4. **Style:** consistent header height (`h-16`/`h-20`)

**✅ Expected Outcome:**  
A reusable, branded header present on all pages.

---

## **Meta Prompt #3: Integrate Global Header into All Pages**

**Objective:**  
Make the new header appear across the app.

### **Instructions**
1. Update `src/App.tsx`:
   - Import `Header`
   - Place `<Header />` inside `BrowserRouter`, outside `Routes`

2. Remove redundant navs:
   - `Dashboard.tsx`, `Holdings.tsx`, `AssetMarketplace.tsx`, `AssetDetail.tsx` (keep back btn)

3. Add top padding (`pt-16` / `pt-20`) for content offset.

**✅ Expected Outcome:**  
Uniform header visible across all routes.

---

## **Meta Prompt #4: Update Landing Page Content (Hero & Features)**

**Objective:**  
Revise hero copy and replace six cards with four themed feature blocks.

### **Instructions**
1. **Hero Section (`src/pages/Landing.tsx`):**
   - `H1:` “**Fractional Ownership of Income Generating Assets**”
   - Subtitle:  
     “Buy a fraction of one of our income generating assets! Joining CHAMBRS gives you access to monthly income, transparent reporting, passive earnings & high returns. Manage it all simply online – No hassle, just income.”
   - Keep CTAs.

2. **Replace features with four cards:**

| # | Title | Icon | Description |
|---|--------|------|-------------|
| 1 | Passive Income | Coins/TrendingUp | Receive regular rental payouts; we manage contracts, you receive income. |
| 2 | Fractional Ownership | Users | Access high-value assets via fractional shares from R100 000. |
| 3 | Ease of Management | CheckCircle | Real-time tracking; CHAMBRS manages contracts, maintenance, compliance. |
| 4 | Secure & Compliant | Lock/Shield | Banking-grade security; CIDB, FICA, POPIA & FSCA compliance. |

3. Update layout: `lg:grid-cols-2`  
4. Use `gradient-brand` for card icons.

**✅ Expected Outcome:**  
Landing shows new hero and four feature cards.

---

## **Meta Prompt #5: Add FAQ Section to Landing Page**

**Objective:**  
Append an accordion FAQ to the bottom of the landing page.

### **Instructions**
1. **Create `src/components/FAQ.tsx`:**
   - Use `@radix-ui/react-accordion` + shadcn UI components.
   - Include 6–8 FAQs:
     - How does fractional ownership work?
     - Minimum investment (R100 000)
     - Returns distribution
     - Withdrawal process
     - Security measures
     - Compliance standards (CIDB, FICA, POPIA, FSCA)
     - How to start
     - Asset types available

2. **Style:**
   - Clean professional design
   - Gradient accents, mobile-friendly
   - Smooth animation

3. **Integrate:**  
   Import `<FAQ />` into `Landing.tsx`, below disclaimer.

**✅ Expected Outcome:**  
Interactive accordion FAQ at the bottom of landing.

---

## **Meta Prompt #6: Final Polish & Responsive Testing**

**Objective:**  
Unify branding and responsiveness site-wide.

### **Instructions**
1. **Logo Enhancement (`Header.tsx`):**
   - Gradient text: `bg-gradient-to-r from-black to-[#3432c7] bg-clip-text text-transparent`
   - `text-2xl font-bold`, add hover effect.

2. **Responsive Checklist:**
   - Header collapses under 768 px
   - FAQ smooth on all breakpoints
   - Feature cards stack to 1 column mobile
   - Navigation functional mobile/desktop
   - Gradient renders correctly

3. **Navigation UX:**
   - “Invest” link = auth guard
   - Smooth scroll for anchors
   - Working sign-out in header

4. **Finishing Touches:**
   - Loading states if needed
   - Consistent spacing
   - Sticky header non-overlapping
   - Uniform gradient across app

**✅ Expected Outcome:**  
A polished, fully responsive CHAMBRS web app with cohesive black-to-purple brand styling.

---

## **Summary**

| # | Area | Focus |
|---|------|--------|
| 1 | **Color Scheme** | Update CSS variables + Tailwind config |
| 2 | **Header** | Sticky global header w/ logo + nav |
| 3 | **Integration** | Apply header across pages |
| 4 | **Landing** | Hero + 4 feature cards |
| 5 | **FAQ** | Accordion section |
| 6 | **Polish** | Responsive tuning + final visual pass |

> Execute sequentially; each builds on the previous for predictable, modular deployment.

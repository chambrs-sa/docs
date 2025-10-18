### **Project Overview**
Build **CHAMBRS** - a South African compliant fractional machinery investment platform with full authentication, onboarding, portfolio management, and asset marketplace capabilities.

---

### **1. AUTHENTICATION & USER MANAGEMENT**

**Landing Page** (`/`):
- Hero section with headline: "Empowering Everyday Investors to Earn Like Institutions"
- Subtext: "Access institutional-grade high capital expenditure assets investments with transparent reporting, secure transactions, and full regulatory compliance."
- Two CTAs: "Get Started" and "Sign In" (both navigate to `/auth`)
- Features grid with 6 cards:
  - **Strong Returns**: Coins icon, rental payouts description
  - **Fractional Ownership**: Users icon, R1,000 minimum investment
  - **Ease of Management**: CheckCircle icon, professional asset management
  - **Real-Time Portfolio Tracking**: TrendingUp icon, live updates
  - **Secure & Compliant**: Lock icon, FICA/POPIA compliance
  - **Banking Integration**: Landmark icon, secure bank partnerships
- Compliance footer with Shield icon: "CHAMBRS operates under South African Financial Sector Conduct Authority (FSCA) regulations..."

**Authentication Page** (`/auth`):
- Tabbed interface with Login and Sign Up forms
- **Login**: Email, password, "Forgot password?" link (opens reset dialog)
- **Sign Up**: Full name, email, password, confirm password fields
- Auto-redirect to dashboard if already authenticated
- After login: Check `profiles.onboarding_completed` - redirect to `/onboarding` if false, `/dashboard` if true
- Sign up redirects to `/onboarding`
- Shield icon branding, "Investor Dashboard" title
- Compliance note: "By signing up, you agree to our FICA and POPIA compliance requirements"

**Password Reset Flow**:
- Dialog on auth page for email input
- Separate `/reset-password` page with password + confirm password fields
- Min 6 characters validation

**Protected Routes**:
- All routes except `/`, `/auth`, `/reset-password` require authentication
- Implement `ProtectedRoute` component checking Supabase session

---

### **2. ONBOARDING FLOW** (`/onboarding`)

**5-Step Progressive Form**:

**Step 0 - Personal Information**:
- Full name, ID/Passport number, phone number, physical address
- Validation using Zod schema

**Step 1 - FICA Documents**:
- FileUpload component for ID document
- FileUpload component for proof of address (utility bill/bank statement < 3 months)
- Upload to Supabase storage bucket `fica-documents`

**Step 2 - Investment Details**:
- **Source of Funds** dropdown: Salary, Business Profits, Inheritance, Sale of Assets, Investment Returns, Other
- Conditional text area if "Other" selected
- **Investment Purpose** dropdown: Wealth Creation, Passive Income, Portfolio Diversification, Retirement, Other
- Conditional text area if "Other" selected

**Step 3 - Compliance**:
- **Risk Disclosure** panel (scrollable, 200px max height):
  - "Investment in machinery and equipment carries inherent risks. Capital is not guaranteed..."
  - Checkbox: "I acknowledge and understand the investment risks"
- **Terms & Conditions** panel:
  - "By accepting these terms, you agree to comply with CHAMBRS platform rules..."
  - Checkbox: "I have read and accept the Terms & Conditions"
- **POPIA Consent** panel:
  - "We process your personal information in accordance with POPIA..."
  - Checkbox: "I consent to CHAMBRS processing my personal information"

**Step 4 - Review**:
- Display all entered information in organized sections
- "Submit Application" button
- On submit:
  - Upload documents to Supabase storage
  - Update profile with `onboarding_completed: true`, `fica_verification_status: 'in_review'`
  - Redirect to `/dashboard`

**Progress Tracking**:
- Progress bar showing completion percentage
- Step labels below progress bar
- Save progress to `profiles.onboarding_step` on each Next click
- Form data persists across steps

---

### **3. DASHBOARD** (`/dashboard`)

**Header Navigation**:
- Logo/Title: "Investor Dashboard"
- Nav links: Dashboard, Holdings, Browse Assets, Settings
- Sign Out button (right-aligned)

**Verification Status Banners** (conditional):
- **Pending/In Review**: Yellow banner - "Your account is pending FICA verification. Some features are restricted..."
- **Rejected**: Red banner with "Resubmit" button → `/onboarding`

**Portfolio Overview** (5 cards in grid):
1. **Total Portfolio Value**: R 245,000 | +12.5% this month (green arrow up)
2. **Cash Balance**: R 18,750 | "Available funds" (dollar icon)
3. **Rental Income**: R 8,420 | "This month" (home icon)
4. **Active Investments**: 8 | "Across 4 asset classes" (wallet icon)
5. **Total Returns**: R 45,230 | +18.5% lifetime (trending up icon)

**Charts Section** (2 columns):
- **Asset Status Pie Chart**:
  - Active: 8 (success color)
  - Inactive: 2 (red/destructive color)
  - Recharts PieChart with legend
- **Monthly Returns Bar Chart**:
  - Last 6 months: May (R6200), Jun (R7100), Jul (R6800), Aug (R8400), Sep (R7600), Oct (R8900)
  - Recharts BarChart with primary color bars

**Recent Activity**:
- 3 activity items with icons:
  1. **Investment Added** (arrow up right, gradient primary bg): Excavator Unit #A401, R 50,000, 2 days ago
  2. **Dividend Received** (arrow down right, success bg): Crane Fleet B, +R 3,450, 5 days ago
  3. **Asset Appreciation** (trending up, gradient primary bg): Bulldozer Portfolio, +8.2%, 1 week ago

**Compliance Notice Card**:
- Info icon on primary background
- "FICA & POPIA Compliance" heading
- "Your account is fully compliant with South African financial regulations. All transactions are encrypted and monitored for your security."

---

### **4. HOLDINGS PAGE** (`/holdings`)

**Same Header Navigation** as Dashboard

**Summary Cards** (3 cards):
1. **Total Holdings Value**: Calculate sum of all assets × ownership %
2. **Monthly Rental Income**: Sum of all rental income
3. **Average Ownership**: Average ownership % across assets

**Search & Filters**:
- Search input (searches name, make, model, location)
- Status filter dropdown: All Status, Active, Inactive, Under Maintenance
- Sort dropdown: Valuation, Ownership %, Year, Rental Income

**Holdings Grid**:
Display 8 placeholder assets with these fields:
- Thumbnail image
- Name (e.g., "Caterpillar 320D Excavator")
- Make, Model, Year
- Asset type badge (e.g., "excavator")
- Location with MapPin icon
- Valuation: R XXX,XXX
- Ownership: XX%
- Rental Income: R X,XXX/month
- Operational Status badge with icon:
  - Active: CheckCircle (green)
  - Inactive: XCircle (red)
  - Under Maintenance: Wrench (yellow)

**Sample Data**:
```javascript
[
  { id: 1, name: "Caterpillar 320D Excavator", make: "Caterpillar", model: "320D", year: 2021, 
    valuation: 850000, ownershipPercent: 12.5, operationalStatus: "active", 
    assetType: "excavator", location: "Johannesburg, Gauteng", 
    rentalIncome: 8500, thumbnailUrl: "..." },
  // ... 7 more similar assets
]
```

---

### **5. ASSET MARKETPLACE** (`/assets`)

**Same Header Navigation**

**Summary Cards** (3 cards):
1. **Total Assets Available**: Count of assets
2. **Average Expected Return**: 14.2% per annum
3. **Total Value**: R 6.1M combined

**Search & Filters**:
- Search input (name, make, location)
- Asset Type filter: All Types, Excavator, Crawler
- Sort by: Valuation, Expected Return, Year

**Asset Cards Grid**:
Display 6 available investment opportunities:
- Asset image (aspect-video)
- Name, Make, Model, Year
- Asset type badge
- Description
- **Valuation**: R XXX,XXX
- **Expected Return**: XX% p.a. (green text)
- **Min. Investment**: R XX,XXX
- Location with MapPin icon
- "View Details" button → `/assets/:id`

**Sample Data**:
```javascript
[
  { id: "1", name: "CAT 320D Hydraulic Excavator", make: "Caterpillar", 
    model: "320D", assetType: "excavator", year: 2021, valuation: 850000,
    location: "Johannesburg, Gauteng", availableShares: 60, 
    minimumInvestment: 50000, expectedReturn: 14.5, status: "active",
    description: "Heavy-duty excavator with low hours, excellent condition" },
  // ... 5 more assets
]
```

---

### **6. ASSET DETAIL PAGE** (`/assets/:id`)

**Header**: Back button to marketplace, same navigation

**Layout**: Two-column (main content + investment sidebar)

**Main Content**:

**Asset Header**:
- Large image
- Name, Make, Model, Year
- Location, Asset Type badge, Status badge

**Specifications Grid**:
- VIN Number
- Acquisition Date
- Operating Hours
- Last Maintenance
- Insurance Expiry
- Condition Rating

**Performance Section**:
- Current Valuation card
- Total Rental Revenue card
- Occupancy Rate card
- Average Monthly Return card

**Valuation History Chart**:
- Line chart showing 12-month valuation trend
- Recharts LineChart

**Maintenance Logs Table**:
- Date, Type, Description, Cost, Performed By columns
- 5 recent maintenance records

**Downloadable Documents**:
- 3 document cards with Download icons:
  - Insurance Certificate
  - Ownership Documents  
  - Safety Certificates
- Mock download functionality (toast notification)

**Investment Sidebar** (sticky):
- Asset Valuation: R XXX,XXX
- Available Shares: XX%
- Minimum Investment: R XX,XXX
- Expected Return: XX% p.a.
- Number input for investment amount
- "Invest Now" button (primary gradient)
- Risk disclaimer text

---

### **7. PROFILE SETTINGS** (`/profile-settings`)

**Navigation**: Back button to dashboard

**Profile Information Card**:
- Full Name input
- Phone Number input
- Address textarea
- "Update Profile" button

**FICA Verification Status**:
- Display status badge: Pending (yellow), Verified (green), In Review (blue), Rejected (red)

**Security Settings**:
- Button: "Change Password" → `/reset-password`

---

### **8. DATABASE SCHEMA**

**profiles table**:
- `id` (uuid, PK, references auth.users)
- `full_name`, `id_number`, `phone_number`, `address`
- `source_of_funds`, `source_of_funds_details`
- `investment_purpose`, `investment_purpose_details`
- `risk_acknowledgment`, `terms_accepted`, `consent_given` (booleans)
- `terms_accepted_date`, `consent_date` (timestamps)
- `onboarding_completed` (boolean, default false)
- `onboarding_step` (integer, default 0)
- `fica_verification_status` (text: 'pending', 'in_review', 'verified', 'rejected')
- `fica_verified_at`, `fica_verified_by` (uuid)
- `verified` (boolean)
- `created_at`, `updated_at`

**fica_documents table**:
- `id` (uuid, PK)
- `user_id` (uuid, FK → profiles)
- `document_type` (text: 'id_document', 'proof_of_address')
- `file_name`, `file_path`, `file_size`, `mime_type`
- `verified` (boolean, default false)
- `verified_at`, `verified_by` (uuid)
- `uploaded_at`, `notes`

**Storage Buckets**:
- `fica-documents` (private)
- `asset-images` (public)

**RLS Policies**:
- Users can read/update their own profiles
- Users can upload their own FICA documents
- Admins/compliance officers can view all profiles and documents

**Trigger**: `handle_new_user()` - Creates profile record on auth.users insert

---

### **9. TECHNICAL REQUIREMENTS**

**Stack**:
- React + TypeScript + Vite
- Tailwind CSS + shadcn/ui components
- React Router DOM (v6)
- Supabase (auth, database, storage)
- Recharts (charts/graphs)
- Zod (form validation)
- React Hook Form + @hookform/resolvers
- Lucide React (icons)
- date-fns (date formatting)

**Authentication Flow**:
- Supabase `signInWithPassword`, `signUp`, `signOut`
- `onAuthStateChange` listener for session management
- Email redirect URL configuration
- Session persistence in localStorage

**File Upload**:
- FileUpload component with drag-and-drop
- Upload to Supabase storage with `uploadFicaDocument()` helper
- Store metadata in `fica_documents` table

**Form Validation**:
- Zod schemas for all onboarding steps
- Real-time error handling with toast notifications

**Styling**:
- Gradient primary: Blue-purple gradient
- Success color for positive metrics
- Destructive/red for inactive/rejected states
- Muted backgrounds for panels
- Consistent card shadows and hover effects

**Navigation**:
- Consistent header across all authenticated pages
- Protected routes checking Supabase session
- Breadcrumb/back navigation on detail pages

---

### **10. KEY USER FLOWS**

1. **New User**: Landing → Sign Up → Onboarding (5 steps) → Dashboard
2. **Returning User**: Landing → Login → Dashboard (or Onboarding if incomplete)
3. **Browse & Invest**: Dashboard → Browse Assets → Asset Detail → Invest
4. **Portfolio Management**: Dashboard → Holdings → View individual assets
5. **Profile Update**: Settings → Update info → Change password

---

This prompt captures all functionality, UI details, data models, and technical specifications to fully recreate the CHAMBRS platform. Copy this prompt into a new Lovable project to replicate the entire application.

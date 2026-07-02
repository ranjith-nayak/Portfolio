# ENGINEERING PORTFOLIO - ARCHITECTURE DOCUMENT

## 1. INFORMATION ARCHITECTURE

```
Root
├── Homepage / Entry Point
│   ├── Hero Section (Brand, Mission)
│   ├── Quick Snapshot (Stats/Summary)
│   └── Navigation Hint
│
├── Workspace (Main Hub)
│   ├── Sidebar Navigation
│   ├── Main Content Area
│   └── Filter/Sort Controls
│
├── Product Section
│   ├── Products Grid/List
│   ├── Product Card (Preview)
│   └── Product Viewer (Deep View)
│
├── Analytics Repository
│   ├── Analytics Projects Grid
│   └── Analytics Project Viewer
│
├── Engineering Labs
│   ├── Experiments/Tools Grid
│   └── Lab Project Viewer
│
├── Skills/Expertise
│   ├── Technical Skills (organized by category)
│   └── Proficiency Visualization
│
├── Journey/Timeline
│   ├── Career Timeline
│   ├── Milestone Cards
│   └── Story Narrative
│
└── Contact/CTA
    ├── Email
    ├── GitHub
    ├── LinkedIn
    ├── Resume Download
    └── Message Form (optional)
```

## 2. FOLDER STRUCTURE

```
portfolio/
│
├── index.html                 # Entry point
├── README.md                  # Documentation
│
├── css/
│   ├── global.css             # Reset, variables, base styles
│   ├── components.css         # Reusable components
│   ├── layout.css             # Layout system (grid, flex)
│   ├── animations.css         # Animation definitions
│   └── responsive.css         # Media queries
│
├── js/
│   ├── main.js                # Initialization, state management
│   ├── navigation.js          # Navigation logic
│   ├── viewer.js              # Product/Project viewer
│   ├── utils.js               # Helper functions
│   └── data/
│       ├── products.js        # Product data (HELIX, SUPPLYSCOPE, etc)
│       ├── projects.js        # Analytics projects data
│       ├── labs.js            # Engineering labs data
│       └── skills.js          # Skills data
│
├── assets/
│   ├── icons/                 # SVG icons (inline in HTML)
│   └── images/                # Minimal static images if needed
│
└── github/
    └── workflows/             # GitHub Actions (optional, for CI/CD)
```

## 3. WIREFRAME (ASCII)

```
┌─────────────────────────────────────────┐
│              HOMEPAGE                   │
├─────────────────────────────────────────┤
│                                         │
│         [HERO SECTION]                  │
│    Welcome to Engineering Workspace     │
│         [Primary CTA]                   │
│                                         │
├─────────────────────────────────────────┤
│   WORKSPACE (Main Navigation Hub)       │
├─────────────────────────────────────────┤
│  [◁ SIDEBAR] │         [MAIN CONTENT]   │
│   • Products │  ┌──────────────────┐    │
│   • Analytics│  │ Product Grid     │    │
│   • Labs     │  │ [Card] [Card]    │    │
│   • Skills   │  │ [Card] [Card]    │    │
│   • Journey  │  │ [Card] [Card]    │    │
│   • Contact  │  │                  │    │
│              │  └──────────────────┘    │
├──────────────┴──────────────────────────┤
│        PRODUCT VIEWER (Modal)           │
│  ╔══════════════════════════════════╗   │
│  ║    Product Details               ║   │
│  ║    Screenshots/Info              ║   │
│  ║    Stats                         ║   │
│  ║    [Close] [GitHub Link]         ║   │
│  ╚══════════════════════════════════╝   │
└─────────────────────────────────────────┘
```

## 4. NAVIGATION FLOW

```
ENTRY POINT
    ↓
[Homepage] ← Display hero, hint to workspace
    ↓
[Click "Enter Workspace"] OR [Auto-transition]
    ↓
[Workspace Hub] ← Shows sidebar + initial content
    ↓
[Select Section] ← Products, Analytics, Labs, etc.
    ↓
[Main content loads via JavaScript]
    ↓
[Click Project Card]
    ↓
[Product Viewer opens] ← Modal/Overlay with details
    ↓
[Close Viewer] OR [Navigate to another project]
    ↓
[Return to section]
    ↓
[Change section via sidebar]
    ↓
[Main content updates smoothly]
    ↓
[No page reloads at any point]
```

## 5. STATE MANAGEMENT PLAN

### Global State (Single Source of Truth)

```javascript
const AppState = {
  // Current view
  currentSection: 'products',      // 'products', 'analytics', 'labs', 'skills', 'journey'
  currentView: 'workspace',        // 'home', 'workspace'
  
  // Viewer state
  viewerOpen: false,
  viewerContent: null,             // Currently displayed project
  viewerType: null,                // 'product', 'analytics', 'labs'
  
  // UI state
  sidebarOpen: true,               // Mobile responsiveness
  filters: {
    category: 'all',               // For filtering projects
    sortBy: 'recent'
  },
  
  // Cache
  projectsCache: {},
  analyticsCache: {},
  labsCache: {}
};
```

### State Update Pattern

```javascript
// Never mutate state directly
function updateState(path, value) {
  setIn(AppState, path, value);
  render();  // Re-render after state change
}

// Usage
updateState('currentSection', 'products');
updateState('viewerOpen', true);
```

### Event Flow

```
User Interaction (Click, Navigate)
    ↓
Event Handler catches it
    ↓
Update AppState via updateState()
    ↓
Render function reads AppState
    ↓
DOM updates (content swaps, animations trigger)
    ↓
No page reload, no hydration needed
```

## 6. COMPONENT TREE

```
App
├── Header
│   ├── Logo
│   ├── Navigation (minimal on homepage)
│   └── Theme Toggle (optional)
│
├── HomePage
│   ├── HeroSection
│   │   ├── Headline
│   │   ├── Subtitle
│   │   └── CTA Button
│   │
│   └── SnaphotSection
│       ├── Stat Card (Products count)
│       ├── Stat Card (Total projects)
│       └── Tech Stack Preview
│
├── Workspace
│   ├── Sidebar
│   │   ├── Section Links
│   │   └── Settings (optional)
│   │
│   └── MainContent
│       ├── SectionHeader
│       │   ├── Title
│       │   └── Filter/Sort Controls
│       │
│       └── ContentArea
│           ├── GridView / ListView (dynamic)
│           ├── ProjectCard
│           │   ├── Thumbnail/Icon
│           │   ├── Title
│           │   ├── Description
│           │   ├── Tags
│           │   └── Meta (date, status)
│           │
│           └── EmptyState (if needed)
│
├── ProductViewer (Modal/Overlay)
│   ├── Header (Title, Close Button)
│   ├── Content
│   │   ├── ProjectDetails
│   │   ├── Screenshots/Stats
│   │   ├── Technologies Used
│   │   └── Links (GitHub, Demo)
│   │
│   └── Footer (Navigation, Close Button)
│
└── Footer
    ├── Contact Links
    ├── Social Links
    └── Copyright
```

## 7. DATA STRUCTURE

### Products Data

```javascript
const PRODUCTS = [
  {
    id: 'helix',
    title: 'HELIX',
    subtitle: 'Enterprise Data Intelligence Platform',
    category: 'product',
    status: 'production',
    releaseDate: '2024',
    
    description: 'Full-stack ETL pipeline with data warehouse, analytical dashboards, and predictive modeling.',
    longDescription: '...',
    
    technologies: ['Python', 'SQL', 'Apache Spark', 'Power BI', 'PostgreSQL'],
    impact: {
      metric1: { label: 'ETL Records', value: '500M+' },
      metric2: { label: 'Processing Speed', value: '10x faster' },
      metric3: { label: 'Query Latency', value: '<500ms' }
    },
    
    images: [],                    // Screenshots (if any)
    highlights: ['Real-time analytics', 'Scalable architecture', 'Production ready'],
    github: 'https://github.com/...',
    demo: null,
    
    features: [
      { name: 'ETL Pipeline', desc: '...' },
      { name: 'Data Warehouse', desc: '...' }
    ]
  },
  // ... more products
];
```

### Analytics Projects Data

```javascript
const ANALYTICS_PROJECTS = [
  {
    id: 'customer-churn',
    title: 'Customer Churn Analytics',
    category: 'analytics',
    status: 'archive',
    
    description: 'Predictive ML model + Power BI dashboard analyzing customer retention.',
    technologies: ['Python', 'Scikit-learn', 'Power BI', 'SQL'],
    
    keyFindings: [
      'Identified 3 churn risk factors',
      'Improved retention by 23%',
      'Saved $2.1M annually'
    ],
    
    github: 'https://github.com/...',
    metrics: {
      accuracy: '94%',
      dataPoints: '50K+ records'
    }
  },
  // ... more projects
];
```

### Skills Data

```javascript
const SKILLS = {
  dataEngineering: {
    label: 'Data Engineering',
    items: ['Python', 'SQL', 'Spark', 'ETL', 'Data Warehousing']
  },
  analytics: {
    label: 'Analytics & BI',
    items: ['Power BI', 'Tableau', 'SQL', 'DAX', 'Data Modeling']
  },
  tools: {
    label: 'Tools & Platforms',
    items: ['PostgreSQL', 'Git', 'Docker', 'Linux', 'AWS']
  },
  // ... more categories
};
```

---

## DESIGN PRINCIPLES FOR IMPLEMENTATION

### Color System
- **Background**: Deep, neutral (avoid pure black)
- **Accent**: Single, sophisticated color (blue, gray, or teal)
- **Text**: High contrast white/off-white on dark
- **Borders**: Subtle, 1px, low opacity

### Typography
- **System fonts**: Apple system stack (-apple-system, BlinkMacSystemFont, etc.)
- **Hierarchy**: 4-5 distinct sizes max
- **Letter spacing**: Subtle, -0.5px to 0.5px
- **Line height**: 1.5-1.6 for body, 1.1-1.3 for headings

### Spacing
- **Base unit**: 8px (8, 16, 24, 32, 48, 64px)
- **Consistency**: Use scale throughout
- **Breathing room**: Generous whitespace

### Animations
- **Duration**: 100-300ms max
- **Easing**: cubic-bezier for smoothness
- **Purposeful**: Every animation communicates something
- **Reduced motion**: Respect prefers-reduced-motion

### Responsive Strategy
- **Desktop first**: Design for 1440px
- **Tablet**: 768px breakpoint
- **Mobile**: 375px breakpoint
- **No horizontal scroll**

---

## RATIONALE: WHY EACH SECTION EXISTS

| Section | Purpose | Communicates |
|---------|---------|--------------|
| **Products** | Core portfolio pieces | Engineering capability |
| **Analytics** | Supporting body of work | Depth and consistency |
| **Labs** | Experiments & tools | Curiosity and learning |
| **Skills** | Skill inventory | Credibility |
| **Journey** | Story and growth | Human side + ambition |
| **Contact** | Conversion point | Accessibility |

---

## VALIDATION CHECKLIST

- [ ] No page reloads
- [ ] All content loads from JS objects (no hardcoding in HTML)
- [ ] Animations under 300ms
- [ ] Mobile responsive without viewport lag
- [ ] Accessible (keyboard nav, ARIA labels)
- [ ] No external frameworks
- [ ] CSS variables for theming
- [ ] Semantic HTML
- [ ] Performance: Lighthouse 90+
- [ ] Works on GitHub Pages

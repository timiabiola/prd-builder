# Responsive UI Architecture

Every app implements desktop-first design with comprehensive mobile optimization.

## Design Priority

1. **Primary**: Desktop/laptop (1024px+)
2. **Secondary**: Tablet (768px-1023px)
3. **Tertiary**: Mobile (< 768px)

Both experiences must be excellent — desktop is the reference design, not an excuse to neglect mobile.

## Mobile Detection Hooks

### `useMobile`

```typescript
// hooks/use-mobile.ts
'use client'

import { useState, useEffect } from 'react'

const MOBILE_BREAKPOINT = 768

export function useMobile() {
  const [isMobile, setIsMobile] = useState<boolean | undefined>(undefined)

  useEffect(() => {
    const mql = window.matchMedia(`(max-width: ${MOBILE_BREAKPOINT - 1}px)`)
    const onChange = () => setIsMobile(window.innerWidth < MOBILE_BREAKPOINT)

    mql.addEventListener('change', onChange)
    setIsMobile(window.innerWidth < MOBILE_BREAKPOINT)

    return () => mql.removeEventListener('change', onChange)
  }, [])

  return !!isMobile
}
```

### `useBreakpoint`

```typescript
// hooks/use-breakpoint.ts
'use client'

import { useState, useEffect } from 'react'

type Breakpoint = 'mobile' | 'tablet' | 'desktop' | 'wide'

const BREAKPOINTS = {
  mobile: 0,
  tablet: 768,
  desktop: 1024,
  wide: 1440,
}

export function useBreakpoint(): Breakpoint {
  const [breakpoint, setBreakpoint] = useState<Breakpoint>('desktop')

  useEffect(() => {
    const getBreakpoint = (width: number): Breakpoint => {
      if (width >= BREAKPOINTS.wide) return 'wide'
      if (width >= BREAKPOINTS.desktop) return 'desktop'
      if (width >= BREAKPOINTS.tablet) return 'tablet'
      return 'mobile'
    }

    const handleResize = () => setBreakpoint(getBreakpoint(window.innerWidth))

    handleResize()
    window.addEventListener('resize', handleResize)
    return () => window.removeEventListener('resize', handleResize)
  }, [])

  return breakpoint
}

export function useIsMobile() {
  return useBreakpoint() === 'mobile'
}

export function useIsTablet() {
  return useBreakpoint() === 'tablet'
}

export function useIsDesktop() {
  const bp = useBreakpoint()
  return bp === 'desktop' || bp === 'wide'
}
```

## Component Usage

```tsx
'use client'

import { useMobile } from '@/hooks/use-mobile'
import { DesktopNav } from './desktop-nav'
import { MobileNav } from './mobile-nav'

export function Navigation() {
  const isMobile = useMobile()
  return isMobile ? <MobileNav /> : <DesktopNav />
}
```

## PRD Responsive UI Section Template

Include this in every PRD:

```markdown
## Responsive UI Requirements

### Desktop Experience (Primary)
- Full navigation sidebar
- Multi-column layouts
- Hover interactions and tooltips
- Keyboard shortcuts
- Data-dense tables and charts

### Tablet Experience
- Collapsible sidebar
- Adapted 2-column layouts
- Touch-optimized controls
- Simplified data views

### Mobile Experience
- Bottom navigation or hamburger menu
- Single-column layouts
- Touch-first interactions (44px+ targets)
- Card-based data presentation
- Pull-to-refresh patterns
- Optimized input types (tel, email, etc.)

### Shared Behaviors
- Consistent branding and typography scale
- Shared animation patterns (reduced motion supported)
- Progressive disclosure of complex features
- Offline-capable where applicable
```

## Tailwind Responsive Pattern

Use mobile-first Tailwind classes with desktop overrides:

```tsx
<div className="
  flex flex-col           /* Mobile: vertical stack */
  md:flex-row             /* Tablet+: horizontal */
  lg:gap-8                /* Desktop: larger gaps */
  p-4 md:p-6 lg:p-8       /* Progressive padding */
">
  <aside className="
    w-full                /* Mobile: full width */
    md:w-64               /* Tablet+: fixed sidebar */
    md:flex-shrink-0
  ">
    {/* Sidebar content */}
  </aside>

  <main className="
    flex-1
    min-w-0               /* Prevent overflow */
  ">
    {/* Main content */}
  </main>
</div>
```

## Performance Budgets

| Metric | Mobile (3G) | Desktop |
|--------|-------------|---------|
| FCP | < 2.5s | < 1.8s |
| TTI | < 5.0s | < 3.9s |
| LCP | < 4.0s | < 2.5s |
| CLS | < 0.1 | < 0.1 |
| Bundle Size | < 200KB JS | < 400KB JS |

## UI Build Workflow

1. **Design**: Use `/frontend-design` skill with clinical elegance light + editorial style
2. **Build**: Desktop-first components with Tailwind responsive utilities + HeroUI + Framer Motion
3. **Optimize**: Use `mobile-ui-optimizer` agent to audit mobile experience
4. **Test**: Verify breakpoints (320px, 375px, 414px, 768px, 1024px, 1440px)

After completing any UI component, proactively invoke the mobile-ui-optimizer agent:

```
Task("Mobile UI Optimizer", "Review and optimize [component/page] for mobile. Check touch targets, responsive behavior, and mobile-specific enhancements.", "mobile-ui-optimizer")
```

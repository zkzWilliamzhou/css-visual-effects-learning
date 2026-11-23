# css-visual-effects-learning - SPEC v0.1.0

## Project Identity

| Field | Value |
|-------|-------|
| **Name** | css-visual-effects-learning |
| **GitHub** | zkzWilliamzhou/css-visual-effects-learning |
| **Description** | Interactive learning platform for CSS/JS visual effects with live preview, gamified progress, and shareable creations |
| **Target Stars** | 8K-15K |
| **License** | MIT |
| **Language** | TypeScript (strict) |

---

## Core Value Proposition

**"Make CSS/JS visual effects learnable by doing, not reading."**

- **Problem**: CSS/JS visual effects (animations, transitions, shaders, canvas effects) are powerful but learning resources are static (articles, videos). CodePen exists but lacks structured curriculum, gamification, and learning progression.
- **Solution**: An interactive platform where learners edit code and see instant visual feedback, progress through curated effects from basic to advanced, earn achievements, and share creations.
- **Unique Angle**: Structured curriculum + live editor + gamified progression + shareable creations + community showcase = viral learning loop.

---

## User Personas

| Persona | Needs | Success Metric |
|---------|-------|----------------|
| **Frontend Beginner** | Visual feedback, guided path, low frustration | Completes 5+ effects in first session |
| **Self-taught Developer** | Structured curriculum, best practices, portfolio pieces | Builds 3+ showcase effects for portfolio |
| **Bootcamp Student** | Assignable exercises, progress tracking, instructor view | 90% completion rate on assigned modules |
| **Experienced Dev** | Advanced effects reference, inspiration, quick prototyping | Uses as reference 2+ times/week |

---

## Feature Specification

### Phase 1 (v0.1.0) - MVP

#### 1. Core Interactive Editor
- **Monaco Editor** embedded (TypeScript/JS/CSS/HTML tabs)
- **Live Preview** iframe with auto-refresh on code change (<100ms latency)
- **Resizable Panels**: Editor (left) / Preview (right) with drag handle
- **Error Overlay**: Syntax errors shown inline in editor + toast in preview
- **Console Capture**: `console.log/error/warn` captured and displayed in UI panel

#### 2. Effect Catalog (10 Effects)
| # | Effect | Category | Difficulty | Learning Objective |
|---|--------|----------|------------|-------------------|
| 1 | Fade In/Out | CSS Animation | ⭐ | `@keyframes`, `animation` |
| 2 | Slide Transition | CSS Transition | ⭐ | `transition`, `transform` |
| 3 | Hover Card Lift | CSS Transform | ⭐ | `transform: translateY`, `box-shadow` |
| 4 | Loading Spinner | CSS Animation | ⭐ | `@keyframes`, `border-radius` |
| 5 | Ripple Button | JS + CSS | ⭐⭐ | Event coords, dynamic elements |
| 6 | Parallax Scroll | JS + CSS | ⭐⭐ | `scrollY`, `transform: translateZ` |
| 7 | Particle System | Canvas API | ⭐⭐⭐ | `requestAnimationFrame`, particle lifecycle |
| 8 | Shader Gradient | WebGL/Canvas | ⭐⭐⭐ | Fragment shader basics |
| 9 | Text Reveal | CSS + JS | ⭐⭐ | `clip-path`, `IntersectionObserver` |
| 10 | Magnetic Button | JS + CSS | ⭐⭐⭐ | Mouse tracking, spring physics |

#### 3. Gamification System
- **XP Points**: 10 XP per completed effect, 50 XP per challenge
- **Levels**: 10 levels (0-9), exponential XP curve
- **Achievements**: 15 badges (e.g., "First Effect", "Animation Master", "Shader Explorer")
- **Streak**: Daily login bonus, 7-day streak = special badge
- **Profile Page**: Stats, badges, completed effects, shared creations

#### 4. Shareable Creations
- **Save to LocalStorage**: Auto-save current editor state
- **Export Link**: Generate unique URL (`/create/:id`) with compressed code
- **Share Modal**: Copy link, Twitter/X share button, download as HTML file
- **Remix Button**: On shared page, "Remix" opens in editor with code pre-loaded

#### 5. Community Showcase (Read-only MVP)
- **Curated Gallery**: Top 20 staff-picked creations
- **Tag Filtering**: By category (CSS, Canvas, WebGL, Text, Interaction)
- **Like Button**: LocalStorage-based likes (no backend)

---

### Phase 2 (v0.2.0) - Enhanced

- **30+ Effects** organized in 3 tracks: CSS Animations, Canvas/JS, WebGL/Shaders
- **Challenge System**: "Recreate this effect" with reference GIF, hints, solution
- **User Accounts**: Optional GitHub OAuth, sync progress across devices
- **Comments**: On shared creations (GitHub Discussions integration)
- **Collections**: User-curated effect collections

---

### Phase 3 (v1.0.0) - Production

- **Full Curriculum**: 50+ effects, learning paths, assessments
- **Instructor Dashboard**: Class management, assignment, progress analytics
- **Mobile App**: React Native / PWA for mobile learning
- **Plugin System**: Community-contributed effect templates
- **Accessibility**: Screen reader support, keyboard navigation, high contrast mode

---

## Technical Architecture

### Stack
- **Frontend**: Vue 3 + TypeScript + Vite
- **Editor**: Monaco Editor (via `@monaco-editor/loader`)
- **Preview**: Sandbox iframe with `srcdoc` or blob URL
- **State**: Pinia + localStorage persistence
- **Styling**: UnoCSS (atomic CSS, zero-runtime)
- **Build**: Vite + Rollup (library mode for embeddable widget)
- **Testing**: Vitest + Playwright (E2E)
- **Docs**: VitePress

### Project Structure
```
css-visual-effects-learning/
├── packages/
│   ├── core/                 # Shared types, utilities, effect registry
│   ├── editor/               # Monaco wrapper, preview iframe, error handling
│   ├── gamification/         # XP, levels, achievements, streak logic
│   ├── effects/              # Effect definitions (JSON + preview code)
│   ├── sharing/              # Compression, URL generation, remix logic
│   └── showcase/             # Gallery components, filtering, liking
├── apps/
│   ├── web/                  # Main VitePress + SPA
│   └── widget/               # Embeddable editor widget (npm package)
├── docs/                     # VitePress documentation
├── tests/
│   ├── unit/                 # Vitest unit tests
│   └── e2e/                  # Playwright E2E tests
├── package.json              # Root workspace (pnpm)
├── turbo.json                # Turborepo config
└── README.md
```

### Effect Definition Schema
```typescript
// packages/effects/src/types.ts
interface Effect {
  id: string;                    // kebab-case, unique
  title: string;                 // Display name
  description: string;           // Learning objective
  category: 'css' | 'canvas' | 'webgl' | 'text' | 'interaction';
  difficulty: 1 | 2 | 3;         // ⭐, ⭐⭐, ⭐⭐⭐
  tags: string[];                // e.g., ['animation', 'hover', 'button']
  estimatedTime: number;         // Minutes
  xpReward: number;              // Base XP
  
  // Editor starting state
  starterCode: {
    html: string;
    css: string;
    js: string;
  };
  
  // Solution code (hidden until "Show Solution")
  solutionCode: {
    html: string;
    css: string;
    js: string;
  };
  
  // Validation: array of checks to mark "Complete"
  validation: ValidationCheck[];
  
  // Hints for challenge mode
  hints?: string[];
}

type ValidationCheck = 
  | { type: 'selector'; selector: string; property: string; value: string }
  | { type: 'function'; name: string; minCalls: number }
  | { type: 'custom'; fn: string /* JS code returning boolean */ };
```

---

## API Contracts (Internal)

### Core Package Exports
```typescript
// packages/core/src/index.ts
export type { Effect, ValidationCheck, UserProgress, Achievement };
export { EffectRegistry } from './registry';
export { validateEffect } from './validation';
export { compressCode, decompressCode } from './compression';
```

### Gamification Package Exports
```typescript
// packages/gamification/src/index.ts
export { 
  calculateLevel, 
  xpForLevel, 
  awardXP, 
  checkAchievements,
  getStreakBonus 
} from './engine';

export { AchievementRegistry } from './achievements';
export type { UserProfile, Achievement } from './types';
```

### Sharing Package Exports
```typescript
// packages/sharing/src/index.ts
export { 
  createShareLink, 
  parseShareLink, 
  downloadAsHTML,
  generateEmbedCode 
} from './links';
```

---

## Data Models

### UserProgress (localStorage)
```typescript
interface UserProgress {
  version: 1;
  userId: string;              // UUID v4, generated on first visit
  xp: number;
  level: number;
  completedEffects: string[];  // Effect IDs
  achievements: string[];      // Achievement IDs
  streak: {
    current: number;
    longest: number;
    lastActive: string;        // ISO date
  };
  createdAt: string;
  updatedAt: string;
}
```

### SharedCreation (URL state)
```typescript
interface SharedCreation {
  id: string;                  // Base62 encoded, 8 chars
  effectId?: string;           // If based on catalog effect
  code: {
    html: string;
    css: string;
    js: string;
  };
  metadata: {
    title: string;
    description: string;
    tags: string[];
    authorId: string;
    createdAt: string;
    likes: number;
  };
}
```

---

## Testing Strategy

### Unit Tests (Vitest) - Target: 90% coverage
- `EffectRegistry`: CRUD, filtering, validation
- `GamificationEngine`: XP calc, level up, achievements, streak
- `CodeCompression`: Round-trip integrity, size limits
- `ValidationChecks`: Selector, function, custom validators
- `ShareLink`: Encoding/decoding, URL safety

### E2E Tests (Playwright)
- **Happy Path**: Load effect → edit code → pass validation → earn XP → share
- **Challenge Mode**: Load challenge → use hints → complete → unlock achievement
- **Remix Flow**: Open shared link → click Remix → editor loads code → modify → save new
- **Responsive**: Mobile viewport, touch interactions
- **Accessibility**: Keyboard navigation, screen reader labels

### Visual Regression
- Effect preview rendering consistency across browsers
- Editor layout at different viewport sizes

---

## Security Considerations

- **No Code Execution on Server**: All code runs in client iframe (sandboxed)
- **Iframe Sandbox**: `allow-scripts allow-same-origin` only (no forms, popups, downloads)
- **Content Security Policy**: Strict CSP on preview iframe
- **XSS Prevention**: DOMPurify on any user-generated HTML in showcase
- **No Backend in MVP**: All data in localStorage / URL params
- **Rate Limiting**: Client-side throttle on share link generation

---

## Performance Budgets

| Metric | Target |
|--------|--------|
| Initial Load (gzipped) | < 200 KB |
| Time to Interactive | < 2s |
| Editor Mount Time | < 500ms |
| Preview Refresh Latency | < 100ms |
| Bundle Size (widget) | < 50 KB gzipped |

---

## Accessibility (WCAG 2.1 AA)

- **Keyboard Navigation**: All editor actions, panel switching, dialogs
- **Screen Reader**: ARIA labels, live regions for preview updates
- **Color Contrast**: 4.5:1 minimum, high contrast mode toggle
- **Focus Management**: Visible focus indicators, logical tab order
- **Reduced Motion**: Respect `prefers-reduced-motion` in animations

---

## CI/CD Pipeline

### GitHub Actions Workflow
```yaml
# .github/workflows/ci.yml
on: [push, pull_request]
jobs:
  lint:
    runs-on: ubuntu-latest
    steps: [checkout, setup-node, pnpm install, pnpm lint]
  typecheck:
    runs-on: ubuntu-latest
    steps: [checkout, setup-node, pnpm install, pnpm typecheck]
  test:
    runs-on: ubuntu-latest
    steps: [checkout, setup-node, pnpm install, pnpm test]
  e2e:
    runs-on: ubuntu-latest
    steps: [checkout, setup-node, pnpm install, pnpm build, pnpm test:e2e]
  build:
    needs: [lint, typecheck, test, e2e]
    runs-on: ubuntu-latest
    steps: [checkout, setup-node, pnpm install, pnpm build]
    if: github.ref == 'refs/heads/main'
```

### Release Process
1. `pnpm version patch|minor|major` - updates version, creates git tag
2. CI builds, tests, publishes to npm (if library packages)
3. GitHub Release auto-generated from CHANGELOG
4. VitePress docs deployed to GitHub Pages

---

## Documentation Structure (VitePress)

```
docs/
├── guide/
│   ├── getting-started.md
│   ├── editor-basics.md
│   ├── gamification.md
│   ├── sharing.md
│   └── creating-effects.md
├── api/
│   ├── core.md
│   ├── editor.md
│   ├── gamification.md
│   ├── effects.md
│   └── sharing.md
├── effects/
│   ├── css-animations.md
│   ├── canvas-effects.md
│   └── webgl-shaders.md
└── contributing.md
```

---

## Review Gates (8+ Rounds)

| Round | Focus | Gate Criteria |
|-------|-------|---------------|
| 1 | Spec Review | This SPEC approved by PM |
| 2 | Architecture | Code structure matches SPEC, no circular deps |
| 3 | Core Implementation | Editor + Preview + 3 effects working |
| 4 | Gamification | XP, levels, achievements functional |
| 5 | Sharing | Create → Share → Remix flow works |
| 6 | Polish | All 10 effects, responsive, accessible |
| 7 | Performance | Budgets met, no memory leaks |
| 8 | Security | CSP, sandbox, XSS prevention verified |
| 9 | Docs | All API docs, guides complete |
| 10 | Release | CI green, changelog, version bumped |

---

## Risk Mitigation

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Monaco Editor bundle size | High | Medium | Dynamic import, CDN load, worker |
| Iframe sandbox restrictions | Medium | High | Test early, fallback to blob URL |
| localStorage quota exceeded | Low | Medium | Compress data, warn user, export/import |
| Browser compatibility (WebGL) | Medium | Low | Graceful degradation, polyfills |
| Scope creep (too many effects) | High | Medium | Strict Phase 1 scope, backlog for v0.2 |

---

## Success Metrics (Post-Launch)

| Metric | 30 Days | 90 Days | 1 Year |
|--------|---------|---------|--------|
| GitHub Stars | 500 | 2,000 | 10,000 |
| Weekly Active Users | 1,000 | 5,000 | 20,000 |
| Effects Completed/Day | 500 | 2,000 | 10,000 |
| Creations Shared | 100 | 500 | 2,000 |
| Avg Session Duration | 5 min | 8 min | 12 min |
| Return Rate (7-day) | 20% | 35% | 45% |

---

## Appendix: Effect Implementation Notes

### Effect 1: Fade In/Out
```css
/* Starter */
.fade { opacity: 1; }

/* Solution */
@keyframes fade {
  from { opacity: 0; }
  to { opacity: 1; }
}
.fade { animation: fade 1s ease-in-out; }
```

### Effect 7: Particle System (Canvas)
```javascript
// Starter - empty canvas
// Solution - particle class, emission, lifecycle, requestAnimationFrame loop
class Particle { /* x, y, vx, vy, life, color */ }
function animate() { /* update, draw, requestAnimationFrame */ }
```

---

**SPEC Status**: DRAFT v0.1 - Pending PM Review Gate 1  
**Next Action**: PM approval → Phase 2 implementation begins
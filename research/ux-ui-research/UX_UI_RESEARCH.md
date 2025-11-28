# UX/UI Research
## Use-What-You-Have Makeup Coach - Design Patterns & User Experience Guide

**Document Version:** 1.0  
**Date:** November 28, 2025  
**Prepared by:** Makeup Coach App Research Team

---

## Executive Summary

This document outlines UX/UI best practices, design patterns, and accessibility standards for the Makeup Coach application. The focus is on creating an intuitive, inclusive, and engaging user experience.

---

## 1. Design Principles

### 1.1 Core Principles

| Principle | Description | Implementation |
|-----------|-------------|----------------|
| **Coach, Not Critic** | Supportive, encouraging tone | Positive feedback, no judgmental language |
| **Progress Over Perfection** | Celebrate improvement | Milestone celebrations, before/after |
| **Inclusive by Default** | Works for everyone | Diverse representation, accessibility |
| **Simplicity First** | Easy to understand | Clear hierarchy, minimal steps |
| **Real Results** | Focus on practical skills | No unrealistic filters |

### 1.2 Design Goals

1. **Reduce cognitive load** - Simple, focused interfaces
2. **Build confidence** - Supportive feedback loops
3. **Enable independence** - Teach transferable skills
4. **Respect diversity** - Inclusive visuals and features
5. **Ensure accessibility** - WCAG 2.1 AA compliance

---

## 2. User Interface Patterns

### 2.1 Onboarding Flow

```
Screen 1: Welcome
- App value proposition
- "Get Started" CTA

Screen 2: Face Analysis (Optional)
- Camera permission request
- Face shape detection
- Skin tone analysis

Screen 3: Skill Level
- Beginner / Intermediate / Advanced
- Self-selection with descriptions

Screen 4: Goals
- Multi-select: Everyday / Professional / Special Occasion
- "What do you want to learn?"

Screen 5: My Kit Setup
- Add products (scan/manual)
- "Skip for now" option

Screen 6: Personalized Dashboard
- First tutorial recommendation
- Quick wins for engagement
```

### 2.2 Navigation Structure

```
[Bottom Navigation]
├── Home (Dashboard)
├── Learn (Tutorials)
├── My Kit (Products)
├── Progress (Gallery)
└── Profile (Settings)
```

### 2.3 Key Screen Patterns

| Screen | Primary Action | Secondary Actions |
|--------|----------------|-------------------|
| Dashboard | Start Tutorial | View Progress, Quick Tips |
| Tutorial List | Select Tutorial | Filter, Search, Save |
| Tutorial Player | Follow Steps | Pause, Replay, Skip |
| My Kit | Add Product | Edit, Delete, Organize |
| Progress | View Timeline | Share, Compare, Delete |

---

## 3. Tutorial Experience Design

### 3.1 Tutorial Player Features

| Feature | Purpose | Implementation |
|---------|---------|----------------|
| **Step-by-step guidance** | Clear progression | Numbered steps, progress bar |
| **Pause/Resume** | User control | Persistent state |
| **Speed control** | Accessibility | 0.5x, 1x, 1.5x, 2x |
| **Mirror mode** | Natural viewing | Flip video horizontally |
| **Voice narration** | Hands-free learning | Text-to-speech |
| **Closed captions** | Accessibility | Always available |
| **Replay step** | Practice | Quick replay button |

### 3.2 Interactive Elements

| Element | Description |
|---------|-------------|
| **Hotspots** | Tap to learn more about specific areas |
| **AR overlay** | Show where to apply on user's face |
| **Timer** | Guide application time (e.g., "blend for 30 seconds") |
| **Checkpoint** | "Ready to move on?" confirmation |
| **Tip cards** | Pro tips that appear contextually |

---

## 4. Visual Design System

### 4.1 Color Palette

| Color | Hex | Usage |
|-------|-----|-------|
| Primary | #E57373 | CTAs, highlights (warm, approachable) |
| Secondary | #64B5F6 | Links, secondary actions |
| Success | #81C784 | Achievements, completion |
| Warning | #FFD54F | Alerts, tips |
| Error | #E57373 | Errors (gentle, not alarming) |
| Neutral Dark | #424242 | Primary text |
| Neutral Light | #F5F5F5 | Backgrounds |

### 4.2 Typography

| Style | Font | Size | Weight | Usage |
|-------|------|------|--------|-------|
| H1 | System Sans | 28px | Bold | Screen titles |
| H2 | System Sans | 22px | SemiBold | Section headers |
| Body | System Sans | 16px | Regular | Content |
| Caption | System Sans | 14px | Regular | Labels, hints |
| Button | System Sans | 16px | SemiBold | CTAs |

### 4.3 Iconography

- **Style:** Rounded, friendly, consistent stroke width
- **Size:** 24px standard, 32px for primary actions
- **Colors:** Match text color or primary accent

---

## 5. Accessibility Standards

### 5.1 WCAG 2.1 AA Requirements

| Requirement | Standard | Implementation |
|-------------|----------|----------------|
| **Color Contrast** | 4.5:1 minimum | Test all color combinations |
| **Touch Targets** | 44x44px minimum | Large, well-spaced buttons |
| **Text Scaling** | Support up to 200% | Responsive typography |
| **Screen Readers** | Full support | Semantic labels, alt text |
| **Keyboard Navigation** | Full support | Focus states, tab order |
| **Motion** | Respect preferences | Reduced motion option |

### 5.2 Inclusive Design Features

| Feature | Benefit | Implementation |
|---------|---------|----------------|
| **High contrast mode** | Low vision users | Theme toggle |
| **Voice control** | Motor disabilities | Voice commands |
| **Haptic feedback** | Hearing impaired | Vibration patterns |
| **Dyslexia-friendly font** | Reading disabilities | OpenDyslexic option |
| **Color-blind modes** | Color vision deficiency | Protanopia, Deuteranopia, Tritanopia |

### 5.3 Camera & Lighting Considerations

| Challenge | Solution |
|-----------|----------|
| Low light conditions | Brightness adjustment, flash option |
| Backlighting | Face detection feedback, guidance |
| Varied skin tones | AI trained on diverse dataset |
| Glasses/accessories | Detection handling, user prompts |

---

## 6. Micro-interactions & Feedback

### 6.1 Feedback Patterns

| Action | Feedback Type | Timing |
|--------|---------------|--------|
| Button tap | Haptic + visual | Immediate |
| Tutorial step complete | Animation + sound | 300ms |
| Achievement unlocked | Celebration animation | 1s |
| Photo saved | Toast notification | 2s display |
| Error | Gentle shake + message | Immediate |

### 6.2 Progress Indicators

| Context | Indicator Type |
|---------|----------------|
| Tutorial progress | Step dots / progress bar |
| Skill level | Badge system with levels |
| Loading | Skeleton screens (not spinners) |
| Upload | Progress percentage |

### 6.3 Celebration Moments

- First tutorial completed
- Week streak achieved
- New skill unlocked
- Before/after saved
- Shared progress

---

## 7. Component Library

### 7.1 Core Components

| Component | Variants | States |
|-----------|----------|--------|
| Button | Primary, Secondary, Text, Icon | Default, Hover, Pressed, Disabled |
| Card | Tutorial, Product, Progress | Default, Selected, Loading |
| Input | Text, Search, Dropdown | Default, Focus, Error, Disabled |
| Modal | Confirmation, Info, Form | Open, Closing |
| Toast | Success, Error, Info, Warning | Appearing, Visible, Dismissing |
| Badge | Skill level, Achievement, New | Default |

### 7.2 Beauty-Specific Components

| Component | Purpose |
|-----------|---------|
| **Face Canvas** | Display face with landmarks |
| **Skin Tone Picker** | Visual tone selection |
| **Product Card** | Display makeup products |
| **Tutorial Player** | Video with controls |
| **Before/After Slider** | Compare images |
| **Progress Timeline** | Visual skill journey |

---

## 8. Competitor UX Analysis

### 8.1 Best Practices from Leaders

| App | Strength | Adopt |
|-----|----------|-------|
| **Sephora** | Product discovery | Visual search |
| **YouCam** | AR try-on smoothness | Real-time preview |
| **Duolingo** | Gamification | Streaks, achievements |
| **Headspace** | Calm onboarding | Supportive tone |
| **Pinterest** | Visual inspiration | Save & organize |

### 8.2 UX Gaps to Address

| Gap | Our Solution |
|-----|-------------|
| Tutorials assume products | Works with user's kit |
| No progress tracking | Visual skill journey |
| One-size-fits-all | Personalized to face/skin |
| Commerce-focused | Education-focused |
| Filters distort reality | Real, achievable results |

---

## 9. Design Recommendations

### 9.1 MVP Priorities

1. Simple, fast onboarding (<2 minutes)
2. Clear tutorial navigation
3. Accessible camera experience
4. Basic progress tracking
5. Product kit management

### 9.2 Future Enhancements

1. Advanced AR overlays
2. Community features
3. Personalized AI recommendations
4. Gamification system
5. Social sharing

---

## References

1. Apple Human Interface Guidelines
2. Material Design 3 Guidelines
3. WCAG 2.1 Accessibility Standards
4. Nielsen Norman Group - Mobile UX
5. Inclusive Design Principles

---

*This document is part of the pre-development research phase for the Use-What-You-Have Makeup Coach application.*

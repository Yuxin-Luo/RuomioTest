# Todo App Specification

## 1. Concept & Vision

**Breeze** — A calming, airy todo app that feels like a breath of fresh air. The interface evokes the serenity of a clear sky with soft cloud-like elements floating on a gentle blue gradient. Every interaction feels light and effortless, with subtle animations that make task management feel meditative rather than tedious.

## 2. Design Language

### Aesthetic Direction
Soft minimalism with depth — clean interfaces with layered shadows and glassmorphism touches. Inspired by modern iOS design with a distinct personality.

### Color Palette
```css
--bg-gradient-start: #e8f4fd;    /* Soft sky blue */
--bg-gradient-end: #d4e9f7;      /* Gentle blue */
--card-bg: rgba(255, 255, 255, 0.85);
--card-border: rgba(255, 255, 255, 0.6);
--primary: #3b82f6;             /* Vibrant blue */
--primary-hover: #2563eb;
--primary-light: #dbeafe;
--text-primary: #1e3a5f;        /* Deep navy */
--text-secondary: #64748b;      /* Slate */
--text-muted: #94a3b8;
--success: #10b981;             /* Emerald */
--danger: #ef4444;              /* Red */
--danger-hover: #dc2626;
--shadow-color: rgba(59, 130, 246, 0.15);
```

### Typography
- **Display**: "Nunito" (Google Fonts) — Rounded, friendly, distinctive
- **Body**: "Nunito" throughout for consistency
- **Weights**: 400 regular, 600 semibold, 700 bold

### Spatial System
- Base unit: 8px
- Card padding: 32px
- Element gaps: 16px
- Border radius: 12px (cards), 10px (buttons/inputs), 6px (small elements)

### Motion Philosophy
- Entrance: Fade up with stagger (300ms base, 50ms stagger)
- Interactions: Quick response (150ms ease-out)
- Completion: Satisfying scale + color transition (200ms)
- Delete: Slide out left + fade (250ms ease-in)

### Visual Assets
- Custom styled checkbox with checkmark SVG
- Lucide-inspired delete icon (trash)
- Decorative cloud shapes as background elements
- Subtle noise texture overlay on background

## 3. Layout & Structure

### Page Architecture
```
┌─────────────────────────────────────┐
│         Floating Clouds (BG)         │
│  ┌───────────────────────────────┐  │
│  │         Header                │  │
│  │    "✦ Breeze" + Tagline       │  │
│  ├───────────────────────────────┤  │
│  │         Stats Bar             │  │
│  │   [N pending] [N completed]   │  │
│  ├───────────────────────────────┤  │
│  │       Input Area              │  │
│  │   [__________________] [Add]  │  │
│  ├───────────────────────────────┤  │
│  │       Task List               │  │
│  │   ○ Task 1              🗑    │  │
│  │   ● Task 2 (done)       🗑    │  │
│  │   ○ Task 3              🗑    │  │
│  └───────────────────────────────┘  │
│                                     │
│         Footer Credit               │
└─────────────────────────────────────┘
```

### Responsive Strategy
- Mobile (< 640px): Full-width card, reduced padding (20px), smaller text
- Tablet/Desktop: Centered card max-width 560px

## 4. Features & Interactions

### Add Task
- Input field with placeholder "What needs to be done?"
- Add button disabled when input empty
- On submit: Task appears at top with slide-down animation
- Input clears after successful add
- Pressing Enter also submits

### Toggle Complete
- Custom checkbox: unchecked circle → filled with checkmark
- On complete: Task text gets strikethrough, opacity reduces to 70%
- Checkbox fills with success color (#10b981)
- Toggle triggers subtle scale bounce (1.05 → 1.0)

### Delete Task
- Trash icon appears on hover (desktop) / always visible (mobile)
- On click: Task slides out left while fading
- After animation (250ms): Task removed from DOM and localStorage

### Stats Bar
- Shows "N pending" and "N completed"
- Updates instantly on any change
- Subtle pulse animation on change

### Empty State
- When no tasks: Show friendly message with cloud illustration
- "No tasks yet. Add one above!"

### Persistence
- All changes immediately saved to localStorage
- On page load: Restore all tasks from localStorage
- Key: "breeze-todos"

## 5. Component Inventory

### Header
- Title: "✦ Breeze" in Nunito Bold 28px
- Subtitle: "Simple task management" in text-secondary, 14px
- Decorative star icon

### Stats Card
- Two inline badges
- Pending: Primary blue background, white text
- Completed: Success green background, white text
- Rounded pill shape (border-radius: 20px)

### Input Group
- Flex row: input + button
- Input: 48px height, full rounded left side, 10px left padding
- Button: 48px height, primary color, full rounded right side
- Focus state: Input gets blue ring (3px)

### Task Item
- Card-like appearance (white bg, subtle shadow)
- States:
  - Default: white bg, normal text
  - Hover: elevated shadow, delete icon visible
  - Completed: strikethrough text, reduced opacity, green checkbox
- Checkbox: 22px, custom styled, left side
- Task text: flex-grow, 16px
- Delete button: 36px circle, danger color on hover

### Delete Button
- Circular, 36px
- Default: transparent, muted icon
- Hover: danger bg, white icon
- Active: scale down slightly

## 6. Technical Approach

### Architecture
- Single HTML file with embedded CSS and JS
- No build tools required
- ES6+ JavaScript (const/let, arrow functions, template literals)

### Data Model
```javascript
{
  id: string,        // timestamp-based unique ID
  text: string,      // task content
  completed: boolean,
  createdAt: number  // timestamp
}
```

### localStorage Schema
```javascript
Key: "breeze-todos"
Value: JSON array of task objects
```

### Event Handling
- Event delegation on task list for efficiency
- Input keyboard events (Enter to submit)
- Checkbox click → toggle
- Delete button click → remove with animation delay
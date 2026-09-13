# Visual Design Guidelines

This section provides detailed guidance on creating beautiful, modern UI components using shadcn-ui and best practices for visual design.

## Design Principles

1. **Consistency**: Use the design system tokens (colors, spacing, typography) throughout
2. **Hierarchy**: Establish clear visual hierarchy through size, weight, and color
3. **Breathing Room**: Generous spacing prevents cluttered interfaces
4. **Interactivity**: Provide visual feedback for all interactive elements
5. **Accessibility**: Ensure proper contrast, labels, and keyboard navigation

## Component Library: shadcn-ui

We use [shadcn-ui](https://ui.shadcn.com) as our component foundation. Key characteristics:
- Components are copied into your codebase (not a dependency)
- Built on Radix UI primitives for accessibility
- Styled with Tailwind CSS
- Fully customizable

### Installing Components

When you use this guide, everything is already installed.

### Core Components to Know

- **Card**: Container for content with header, content, footer sections
- **Button**: Primary interactive element with variants (default, outline, ghost, destructive, link)
- **Badge**: Small status or count indicators
- **Separator**: Visual divider between sections
- **Input, Label**: Form elements
- **Dropdown Menu, Dialog**: Overlay components

## Icon Library: Lucide React

Use [Lucide React](https://lucide.dev) for all icons:

```typescript
import { Trash2, ExternalLink, BookMarked, Download } from 'lucide-react';
```

**Icon Guidelines**:
- Use semantic icons that clearly represent their action
- Standard size: `h-4 w-4` for inline with text, `h-5 w-5` for standalone buttons
- Large decorative: `h-6 w-6` or `h-12 w-12` for empty states
- Always include icons with buttons for important actions

## Layout Patterns

### Container Structure

```typescript
<div className="min-h-screen bg-background">
  <div className="container max-w-5xl mx-auto py-8 px-4 sm:px-6 lg:px-8">
    {/* Content */}
  </div>
</div>
```

- `min-h-screen`: Full viewport height
- `container max-w-5xl mx-auto`: Centered with max width for readability
- Responsive padding: `px-4 sm:px-6 lg:px-8`

### Header Pattern

```typescript
<header className="space-y-6 mb-8">
  <div className="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
    <div className="flex items-center gap-3">
      <div className="p-2 bg-primary/10 rounded-lg">
        <BookMarked className="h-6 w-6 text-primary" />
      </div>
      <div>
        <h1 className="text-3xl font-bold tracking-tight">Page Title</h1>
        <p className="text-muted-foreground mt-1">
          Descriptive subtitle
        </p>
      </div>
    </div>
    <div className="flex items-center gap-3">
      {/* Actions like badges, buttons */}
    </div>
  </div>
  <Separator />
</header>
```

**Key elements**:
- Icon in colored background circle for visual interest
- Title with subtitle for context
- Responsive flex layout (column on mobile, row on desktop)
- Actions aligned to the right
- Separator for visual hierarchy

## Card Design Patterns

### List Item Card

```typescript
<Card className="group hover:shadow-lg transition-shadow duration-200">
  <CardContent className="p-6">
    <div className="flex items-start justify-between gap-4">
      <div className="flex-1 space-y-3">
        <CardTitle className="text-2xl font-bold text-primary">
          Primary Content
        </CardTitle>
        <CardDescription className="text-base leading-relaxed">
          Secondary descriptive text with good line height
        </CardDescription>
        {/* Additional metadata */}
      </div>
      <Button
        variant="ghost"
        size="icon"
        className="text-muted-foreground hover:text-destructive transition-colors shrink-0"
      >
        <Trash2 className="h-5 w-5" />
      </Button>
    </div>
  </CardContent>
</Card>
```

**Key features**:
- `group` class enables group-hover effects
- `hover:shadow-lg transition-shadow` for subtle interaction feedback
- `flex-1` on content allows it to grow, `shrink-0` on button prevents squishing
- `space-y-3` for consistent vertical spacing
- Action buttons use `ghost` variant to reduce visual weight

### Empty State Pattern

```typescript
<div className="flex flex-col items-center justify-center py-20 px-4 text-center">
  <div className="rounded-full bg-muted p-6 mb-6">
    <BookOpen className="h-12 w-12 text-muted-foreground" />
  </div>
  <h3 className="text-xl font-semibold mb-2">No items yet</h3>
  <p className="text-muted-foreground max-w-md">
    Helpful guidance on what the user should do next
  </p>
</div>
```

**Key features**:
- Centered layout with generous padding
- Large icon in circular muted background
- Clear heading and helpful description
- `max-w-md` prevents text from being too wide

## Typography Scale

```typescript
// Page titles
<h1 className="text-3xl font-bold tracking-tight">

// Section headings
<h2 className="text-2xl font-bold">

// Card titles
<h3 className="text-xl font-semibold">

// Body text
<p className="text-base leading-relaxed">

// Metadata/secondary text
<p className="text-sm text-muted-foreground">
```

## Color Usage

Use semantic color tokens, never hardcoded colors:

- **Primary**: `text-primary`, `bg-primary` - Main brand color, important elements
- **Muted**: `text-muted-foreground`, `bg-muted` - Secondary text, backgrounds
- **Destructive**: `text-destructive`, `hover:text-destructive` - Delete, dangerous actions
- **Background**: `bg-background` - Main page background
- **Card**: `bg-card` - Card backgrounds

**Opacity modifiers** for subtle effects:
- `bg-primary/10` - 10% opacity primary color for subtle backgrounds
- `hover:bg-accent` - Subtle hover states

## Spacing System

Use Tailwind's spacing scale consistently:

- **Component internal spacing**: `p-6` (padding), `space-y-3` (vertical gaps)
- **Between components**: `gap-3`, `gap-4` for flex/grid
- **Section spacing**: `mb-8` (margin bottom), `py-8` (vertical padding)
- **List spacing**: `space-y-4` between list items

## Button Patterns

```typescript
// Primary action
<Button className="gap-2">
  <Download className="h-4 w-4" />
  Action Text
</Button>

// Secondary action
<Button variant="outline">
  Secondary Action
</Button>

// Destructive action
<Button variant="ghost" className="hover:text-destructive">
  <Trash2 className="h-5 w-5" />
</Button>

// Icon-only button
<Button variant="ghost" size="icon" aria-label="Delete">
  <Trash2 className="h-5 w-5" />
</Button>
```

**Button guidelines**:
- Use `gap-2` for icon + text spacing (automatic, no margin needed)
- Include `aria-label` for icon-only buttons
- Use appropriate variants: default (primary), outline (secondary), ghost (tertiary)
- Disable state: `disabled={condition}` - automatically styled

## Link Patterns

```typescript
// External link with icon
<a
  href={url}
  target="_blank"
  rel="noopener noreferrer"
  className="inline-flex items-center gap-2 text-sm text-muted-foreground hover:text-primary transition-colors"
>
  <ExternalLink className="h-4 w-4" />
  <span className="underline underline-offset-4">Link text</span>
</a>
```

**Link guidelines**:
- Use `inline-flex items-center gap-2` for icon + text
- `transition-colors` for smooth hover effect
- `underline-offset-4` for better readability
- Always include `rel="noopener noreferrer"` for external links

## Responsive Design

Use mobile-first responsive classes:

```typescript
// Flex direction changes
<div className="flex flex-col sm:flex-row">

// Responsive padding
<div className="px-4 sm:px-6 lg:px-8">

// Responsive text sizes
<h1 className="text-2xl sm:text-3xl">

// Responsive gaps
<div className="gap-2 sm:gap-4">
```

**Breakpoints**:
- `sm:` - 640px (tablet)
- `md:` - 768px (small desktop)
- `lg:` - 1024px (desktop)

## Transition and Animation

Add subtle transitions for better UX:

```typescript
// Hover effects
className="hover:shadow-lg transition-shadow duration-200"
className="hover:text-primary transition-colors"

// Group hover (parent hover affects child)
<div className="group">
  <div className="group-hover:opacity-100 opacity-0 transition-opacity">
```

**Guidelines**:
- Use `duration-200` for quick interactions (hover, focus)
- `transition-shadow` for elevation changes
- `transition-colors` for color changes
- Keep animations subtle and purposeful

## Accessibility Checklist

- [ ] All buttons have clear text or `aria-label`
- [ ] Interactive elements have hover/focus states
- [ ] Color contrast meets WCAG AA standards (handled by design tokens)
- [ ] Form inputs have associated labels
- [ ] Icons are decorative (text provides meaning) or have labels
- [ ] Keyboard navigation works (tab order is logical)

## Common Patterns Reference

**Badge with count**:
```typescript
<Badge variant="secondary" className="h-8 px-3">
  {count} {count === 1 ? 'item' : 'items'}
</Badge>
```

**Disabled state**:
```typescript
<Button disabled={items.length === 0}>
  Export
</Button>
```

**Conditional rendering**:
```typescript
{items.length === 0 ? (
  <EmptyState />
) : (
  <ItemList />
)}
```

## Example: Complete Component

Here's a complete example showing all principles:

```typescript
import type { Word } from '@/models/Word';
import { Card, CardContent, CardDescription, CardTitle } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { ExternalLink, Trash2 } from 'lucide-react';

export const WordListItemView = (props: {
  word: Word;
  onDelete: (id: string) => void;
}) => (
  <Card className="group hover:shadow-lg transition-shadow duration-200">
    <CardContent className="p-6">
      <div className="flex items-start justify-between gap-4">
        <div className="flex-1 space-y-3">
          <CardTitle className="text-2xl font-bold text-primary">
            {props.word.text}
          </CardTitle>
          <CardDescription className="text-base leading-relaxed">
            {props.word.context}
          </CardDescription>
          <a
            href={props.word.sourceUrl}
            target="_blank"
            rel="noopener noreferrer"
            className="inline-flex items-center gap-2 text-sm text-muted-foreground hover:text-primary transition-colors"
          >
            <ExternalLink className="h-4 w-4" />
            <span className="underline underline-offset-4">View source</span>
          </a>
        </div>
        <Button
          variant="ghost"
          size="icon"
          onClick={() => props.onDelete(props.word.id)}
          className="text-muted-foreground hover:text-destructive transition-colors shrink-0"
          aria-label="Delete word"
        >
          <Trash2 className="h-5 w-5" />
        </Button>
      </div>
    </CardContent>
  </Card>
);
```

This example demonstrates:
- ✅ Proper component structure with shadcn-ui
- ✅ Semantic HTML and TypeScript types
- ✅ Consistent spacing and typography
- ✅ Hover effects and transitions
- ✅ Accessible button with aria-label
- ✅ Responsive layout with flex
- ✅ Icon usage with proper sizing
- ✅ Color tokens for theming

# CSS Spacing Cheat Sheet

## Core Spacing Scale

    :root {
        --space-1: 4px;
        --space-2: 8px;
        --space-3: 12px;
        --space-4: 16px;
        --space-5: 24px;
        --space-6: 32px;
        --space-7: 48px;
        --space-8: 64px;
    }

## Padding Rules

### Card Padding

    .card {
        padding: var(--space-4);
    }

### Large Card Padding

    .card-lg {
        padding: var(--space-5);
    }

### Button Padding

    button {
        padding: var(--space-2) var(--space-4);
    }

## Margin Rules

### Heading Spacing

    h1, h2, h3, h4 {
        margin-bottom: var(--space-3);
    }

### Paragraph Spacing

    p {
        margin-bottom: var(--space-3);
    }

### Section Spacing

    .section {
        margin-bottom: var(--space-7);
    }

## Flex / Grid Gaps

    .gap-sm { gap: var(--space-2); }
    .gap-md { gap: var(--space-4); }
    .gap-lg { gap: var(--space-6); }

## Vertical Rhythm

    .stack > * + * {
        margin-top: var(--space-3);
    }

## Mobile Adjustments

    @media (max-width: 600px) {
        :root {
            --space-7: 40px;
            --space-8: 56px;
        }
    }

## Quick Reference

-   4px → tiny adjustments\
-   8px → default gap\
-   16px → card padding\
-   24px → section header spacing\
-   32px → large gap\
-   48px → section spacing\
-   64px → hero/banner spacing

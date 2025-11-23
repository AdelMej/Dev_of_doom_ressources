# CSS Typography Cheat Sheet

## Font Size Scale (rem-based)

    :root {
        --fs-h1: 2.4rem;
        --fs-h2: 2rem;
        --fs-h3: 1.6rem;
        --fs-h4: 1.3rem;
        --fs-body: 1rem;
        --fs-small: 0.9rem;
        --fs-tiny: 0.8rem;
    }

## Headings

    h1, h2, h3, h4 {
        margin: 0 0 12px 0;
        line-height: 1.2;
        font-weight: 700;
    }
    h1 { font-size: var(--fs-h1); }
    h2 { font-size: var(--fs-h2); }
    h3 { font-size: var(--fs-h3); }
    h4 { font-size: var(--fs-h4); }

## Body Text

    p {
        font-size: var(--fs-body);
        line-height: 1.55;
        margin-bottom: 12px;
    }
    small {
        font-size: var(--fs-small);
    }

## Mobile Scaling

    @media (max-width: 600px) {
        :root {
            --fs-h1: 2rem;
            --fs-h2: 1.7rem;
            --fs-h3: 1.4rem;
        }
    }

## Letter Spacing

    h1, h2 { letter-spacing: -0.5px; }
    h3, h4 { letter-spacing: -0.25px; }
    .uppercase { letter-spacing: 1px; }

## UI Text Patterns

### Section Titles

    .section-title {
        font-size: var(--fs-h2);
        font-weight: 700;
        margin-bottom: 20px;
    }

### Card Titles

    .card-title {
        font-size: 1.2rem;
        font-weight: 600;
        margin-bottom: 6px;
    }

### Muted Text

    .text-muted {
        color: var(--text-muted, #777);
    }

### Button Text

    button {
        font-size: var(--fs-body);
        font-weight: 600;
        letter-spacing: 0.3px;
    }

## Spacing System

    .heading-spacing { margin-bottom: 16px; }
    .paragraph-spacing { margin-bottom: 12px; }
    .section-spacing { margin-bottom: 40px; }

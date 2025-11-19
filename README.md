# ForestryComponents

Community-driven component templates for [Forestry.md](https://forestry.md) digital gardens.

## What is this?

This repository contains reusable Nunjucks templates that Forestry.md users can install on their pages. Each template is customizable through a simple form interface - no coding required.

## Using Templates

1. Go to your Forestry.md dashboard
2. Navigate to **Components** → **Template Gallery**
3. Browse and select a template
4. Configure the options to your liking
5. Choose where to place it on your page
6. Click **Install**

That's it! Your component will appear on your next build.

## Available Templates

### Engagement
- **Disqus Comments** - Add comment sections to your notes

### Navigation
- *Coming soon*

### Footer
- *Coming soon*

### Sidebar
- *Coming soon*

## Contributing

We welcome contributions! Here's how to add your own template:

### 1. Fork this repository

### 2. Create your template

Add a `.njk` file in the appropriate category folder:

```
templates/
  engagement/
  navigation/
  footer/
  sidebar/
  utilities/
```

### 3. Template Structure

Every template needs YAML frontmatter:

```njk
---
title: "Your Template Name"
description: "Brief description of what it does"
version: "1.0.0"
author: "Your Name"
category: "Category"
defaultSlot: "header"
variables:
  myVariable:
    type: string
    default: "Default value"
    title: "Display Name"
    description: "Help text for users"
---

<div>
  {{ myVariable }}
</div>
```

### 4. Variable Types

| Type | Description | Form Input |
|------|-------------|------------|
| `string` | Text value | Text input |
| `number` | Numeric value | Number input |
| `boolean` | True/false | Checkbox |
| `select` | Single choice | Dropdown |
| `multiselect` | Multiple choices | Checkboxes |
| `color` | Hex color | Color picker |
| `url` | URL value | URL input |
| `date` | Date value | Date picker |
| `richtext` | Long text | Textarea |

### 5. Available Slots

| Slot ID | Location |
|---------|----------|
| `header` | Page header |
| `footer` | Page footer |
| `before-content` | Before main content |
| `after-content` | After main content |
| `head` | HTML head (meta, styles) |
| `sidebar-top` | Top of sidebar |
| `sidebar-bottom` | Bottom of sidebar |
| `notes-header` | Notes page header |
| `notes-footer` | Notes page footer |
| `notes-before-content` | Before note content |
| `notes-after-content` | After note content |
| `index-header` | Index page header |
| `index-footer` | Index page footer |

### 6. Submit a Pull Request

- Provide a clear description of your template
- Include a screenshot if possible
- Make sure your template works with different variable values

## Guidelines

### Do
- Include clear titles and descriptions for all variables
- Provide sensible default values
- Use scoped CSS (unique class names)
- Consider accessibility
- Test with different configurations

### Don't
- Include external scripts that track users without disclosure
- Use inline styles that override user themes excessively
- Require secret variables, as they will be exposed to the client
- Include minified/obfuscated code

## Fixed UI Stacking System

When creating fixed-position UI components (like floating action buttons), use the `forestry-fixed-ui` system to automatically stack with other fixed components.

### How to Use

1. Add the `forestry-fixed-ui` class to your button
2. Add `data-fixed-position` attribute with the position value
3. Add `data-button-size` attribute with the button size
4. Include the stacking JavaScript in your component

### Example Implementation

```njk
<button class="my-button my-button-{{ position }} forestry-fixed-ui"
        data-fixed-position="{{ position }}"
        data-button-size="{{ buttonSize }}">
    <!-- button content -->
</button>

<script>
(function() {
    const btn = document.currentScript.previousElementSibling.previousElementSibling;
    const position = '{{ position }}';

    // Auto-stack with other forestry-fixed-ui elements in same position
    if (position === 'bottom-right' || position === 'top-right') {
        const siblings = document.querySelectorAll('.forestry-fixed-ui[data-fixed-position="' + position + '"]');
        let offset = 1; // rem

        for (const sibling of siblings) {
            if (sibling === btn) break;
            const siblingSize = sibling.dataset.buttonSize || '2.5rem';
            const sizeValue = parseFloat(siblingSize) || 2.5;
            offset += sizeValue + 0.5; // button size + gap
        }

        if (offset > 1) {
            if (position === 'bottom-right') {
                btn.style.bottom = offset + 'rem';
            } else {
                btn.style.top = offset + 'rem';
            }
        }
    }
})();
</script>
```

### How It Works

- Components query for all `.forestry-fixed-ui` elements with the same `data-fixed-position`
- Each component calculates its offset based on elements that appear before it in the DOM
- Stacking order is determined by DOM order (first component is at base position, subsequent ones stack above)

This allows multiple fixed-position components to coexist without overlapping, and new components can be added without modifying existing ones.

## Template Example

Here's a complete example:

```njk
---
title: "Simple Footer"
description: "A minimal footer with copyright and optional links"
version: "1.0.0"
author: "Forestry.md"
category: "Footer"
defaultSlot: "footer"
variables:
  copyrightName:
    type: string
    default: "My Digital Garden"
    title: "Copyright Name"
    description: "Name shown in copyright notice"
  showYear:
    type: boolean
    default: true
    title: "Show Year"
    description: "Include current year in copyright"
  links:
    type: multiselect
    default:
      - "Home"
      - "About"
    title: "Footer Links"
    description: "Links to show in footer"
---

<footer class="simple-footer">
  <nav>
    {% for link in links %}
    <a href="/{{ link | lower }}">{{ link }}</a>
    {% endfor %}
  </nav>
  <p>
    &copy; {% if showYear %}{{ "now" | date("Y") }}{% endif %} {{ copyrightName }}
  </p>
</footer>

<style>
.simple-footer {
  padding: 2rem;
  text-align: center;
  border-top: 1px solid #eee;
}

.simple-footer nav {
  margin-bottom: 1rem;
}

.simple-footer nav a {
  margin: 0 0.5rem;
  color: #666;
}
</style>
```

## Global Variables

Templates have access to these global variables:

- `{{ globals.pageName }}` - The user's page name

## Questions?

- Open an issue in this repository
- Join the Forestry.md community discussions

## License

MIT License - feel free to use, modify, and share these templates.

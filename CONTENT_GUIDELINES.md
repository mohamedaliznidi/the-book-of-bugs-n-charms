# Content Creation Guidelines

This document establishes the standards and guidelines for creating new content in the frontend development documentation, based on the existing high-quality examples.

## Documentation Structure Standards

### File Organization
- Use descriptive filenames (e.g., `fundamentals.md`, `getting-started.md`, `app-router.md`)
- Avoid generic names like `untitled.md` or `page-1.md`
- Group related content in appropriate directories
- Update `SUMMARY.md` with proper navigation links

### Frontmatter Format
All documentation files should include frontmatter with a description:

```yaml
---
description: >-
  A clear, concise description of what this section covers, 
  written in a way that helps users understand the content's purpose.
---
```

## Content Structure Template

Based on the high-quality examples like `react/react-hooks/usestate.md`, each section should follow this structure:

### 1. Title and Introduction
```markdown
# Section Title

## Introduction

A clear explanation of what the topic is and why it's important. This should be 2-3 sentences that give context and set expectations.
```

### 2. Use Cases (when applicable)
```markdown
## Use Cases

1. **Primary Use Case**: Brief description of the main scenario
2. **Secondary Use Case**: Description of additional scenarios
3. **Advanced Use Case**: More complex implementations
```

### 3. Basic Examples
```markdown
## Basic Example

Provide a simple, working example that demonstrates the core concept:

```javascript
// Clear, commented code example
const example = () => {
    // Explanation of what this does
    return "result";
};
```

Explanation of the example and what makes it work.
```

### 4. Advanced Examples (when applicable)
```markdown
## Advanced Usage

More complex examples that show real-world applications:

```javascript
// More sophisticated example
const advancedExample = (param) => {
    // Complex logic with explanations
    return processedResult;
};
```
```

### 5. Best Practices
```markdown
## Best Practices

- **Practice 1**: Clear explanation of why this is important
- **Practice 2**: Another important guideline
- **Practice 3**: Additional recommendations
```

### 6. Common Pitfalls/Limitations
```markdown
## Common Pitfalls

### Issue 1: Description of Problem
Explanation of what can go wrong and why.

**Solution:**
How to avoid or fix this issue.

### Issue 2: Another Common Problem
Description and solution.
```

### 7. References
```markdown
## References

{% embed url="https://official-documentation-link.com" %}

{% embed url="https://additional-resource.com" %}
```

## Code Standards

### Code Block Formatting
- Always specify the language: ```javascript, ```css, ```html, ```typescript
- Include comments explaining complex logic
- Use realistic, practical examples
- Ensure all code examples are functional and current

### Example Quality Standards
Based on `css/selectors.md` and `css/transitions.md`:

```css
/* Good: Clear, practical example */
.button {
    /* Transition for smooth hover effect */
    transition: all 0.3s ease-in-out;
    background-color: #007bff;
}

.button:hover {
    background-color: #0056b3;
    transform: translateY(-2px);
}
```

## Writing Style Guidelines

### Tone and Voice
- **Beginner-friendly**: Explain concepts clearly without assuming prior knowledge
- **Practical**: Focus on real-world applications and examples
- **Comprehensive**: Cover the topic thoroughly but concisely
- **Professional**: Maintain a professional but approachable tone

### Explanation Depth
Follow the comprehensive style demonstrated in `react/react-hooks/usestate.md`:
- Start with basic concepts
- Build up to more complex scenarios
- Include practical examples throughout
- Address common questions and issues

### Technical Accuracy
- Ensure all code examples work with current versions
- Reference official documentation
- Include version-specific information when relevant
- Test examples before publishing

## Modern Frontend Focus

### Required Modern Topics
When creating new content, prioritize these modern frontend development practices:

1. **React Patterns**
   - Hooks (all major hooks)
   - Server Components
   - Suspense and Error Boundaries
   - Modern state management

2. **TypeScript Integration**
   - Type annotations
   - Interfaces and generics
   - Advanced type patterns
   - Framework-specific typing

3. **Modern CSS**
   - CSS Grid and Flexbox
   - Container queries
   - CSS custom properties
   - Modern layout techniques

4. **Build Tools and Performance**
   - Vite, Webpack, and modern bundlers
   - Code splitting and lazy loading
   - Performance optimization
   - Modern deployment practices

5. **Testing and Quality**
   - Modern testing frameworks (Vitest, Testing Library)
   - Type checking
   - Linting and formatting
   - Accessibility testing

## Quality Checklist

Before publishing new content, ensure:

- [ ] Frontmatter is properly formatted with clear description
- [ ] Introduction explains the topic and its importance
- [ ] Code examples are tested and functional
- [ ] Examples progress from basic to advanced
- [ ] Common pitfalls and solutions are addressed
- [ ] References to official documentation are included
- [ ] Content follows the established structure template
- [ ] Writing is clear and beginner-friendly
- [ ] Modern best practices are emphasized
- [ ] Navigation in SUMMARY.md is updated

## Examples of High-Quality Content

Reference these existing files as examples of the expected quality:

1. **`react/react-hooks/usestate.md`** - Comprehensive explanation with examples, advantages, limitations, and troubleshooting
2. **`css/selectors.md`** - Clear categorization with practical examples
3. **`css/transitions.md`** - Practical guidelines with real-world applications
4. **`css/units-and-functions.md`** - Good balance of explanation and examples
5. **`angular/rxjs-reactive-programming.md`** - In-depth technical content with clear explanations

These examples demonstrate the level of detail, practical focus, and comprehensive coverage expected in all new content.

# Product Page - Product Requirements Document (PRD)

## 1. Overview

**Project Name:** E-Commerce Product Page  
**Version:** 1.0.0  
**Last Updated:** 2026-05-04  
**Status:** In Development

### Purpose
Build a lightweight, accessible, and secure product listing page that displays products from a JSON data source, formats pricing as currency, and allows users to add items to their cart.

### Target Users
- End consumers browsing products
- Users with accessibility needs (screen readers, keyboard navigation)
- Users on slow network connections or mobile devices

---

## 2. Feature Requirements

### 2.1 Core Features

| Feature | Description | Priority |
|---------|-------------|----------|
| Product Display | Show product title, image, description, and price in a grid layout | High |
| Dynamic Data Loading | Fetch product data from ProductsList.json | High |
| Currency Formatting | Display prices in USD format (e.g., $129.99) | High |
| Add to Cart | Button to add products to cart with success notification | High |
| Responsive Design | Mobile, tablet, and desktop support | High |
| Error Handling | Graceful handling of failed data loads | Medium |
| Hover Effects | Visual feedback on product cards and buttons | Medium |

### 2.2 Non-Functional Requirements

| Requirement | Target | Rationale |
|-------------|--------|-----------|
| Page Load Time | < 3 seconds | Better UX and SEO ranking |
| Accessibility Score (WCAG) | AA or higher | Legal compliance & inclusivity |
| Performance Score (Lighthouse) | > 90 | Mobile-first indexing |
| Browser Support | Chrome, Firefox, Safari, Edge (latest 2 versions) | Market coverage |

---

## 3. Coding Standards

### 3.1 HTML Standards

#### Structure & Semantics
- ✅ Use semantic HTML5 elements: `<header>`, `<main>`, `<section>`, `<article>`, `<footer>`
- ✅ Use proper heading hierarchy: `<h1>` → `<h2>` → `<h3>` (no skipping levels)
- ✅ Every image must have descriptive `alt` text
- ✅ Use `<label>` elements for all form inputs
- ✅ Use `<button>` for interactive elements (not `<div>` or `<span>`)

#### Accessibility (WCAG 2.1 AA)
```html
<!-- ✅ Good: Semantic, accessible -->
<img src="product.jpg" alt="Wireless Headphones with noise cancellation">
<button aria-label="Add Wireless Headphones to cart">Add to Cart</button>

<!-- ❌ Bad: Non-semantic, inaccessible -->
<img src="product.jpg">
<div class="button" onclick="addCart()">Add</div>
```

#### Formatting Standards
- Use **2-space indentation** (not tabs)
- One element per line (except inline elements)
- Always include `<!DOCTYPE html>` and language attribute
- Include `<meta charset="UTF-8">` and viewport meta tag

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Product Page</title>
</head>
<body>
  <!-- Content -->
</body>
</html>
```

---

### 3.2 CSS Standards

#### Organization
- **Mobile-first approach**: Base styles for mobile, then add media queries for larger screens
- **Use CSS Grid/Flexbox** over floats
- **Avoid inline styles**: All styles in `<style>` or external CSS file
- **Color variables**: Use CSS custom properties for reusable values

```css
:root {
  --primary-color: #2196F3;
  --primary-hover: #1976d2;
  --text-dark: #333;
  --text-light: #666;
  --border-radius: 8px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
}

.button {
  background-color: var(--primary-color);
  border-radius: var(--border-radius);
  padding: var(--spacing-sm) var(--spacing-md);
}
```

#### Formatting Standards
- Use **2-space indentation**
- Selector names: `kebab-case` (e.g., `.product-card`, `.add-to-cart-btn`)
- One property per line
- Alphabetical property ordering (optional but recommended)

```css
.product-card {
  background-color: white;
  border-radius: var(--border-radius);
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
  padding: 20px;
  transition: transform 0.3s ease;
}
```

#### Accessibility in CSS
- ✅ Maintain 4.5:1 color contrast ratio for text
- ✅ Use `focus` states for keyboard navigation
- ✅ Avoid `color` as the only indicator (use text + icons)
- ✅ Don't remove focus outlines (unless replaced with custom ones)

```css
/* ✅ Good: Clear focus state */
.button:focus {
  outline: 2px solid var(--primary-color);
  outline-offset: 2px;
}

/* ❌ Bad: No focus state */
.button:focus {
  outline: none;
}
```

#### Performance
- Use shorthand properties: `margin: 10px 20px;` instead of individual properties
- Minimize selector nesting (max 3 levels)
- Use `transform` and `opacity` for animations (GPU-accelerated)
- Avoid expensive properties in hover states (e.g., `box-shadow`)

---

### 3.3 JavaScript Standards

#### Code Quality & Security
- ✅ Use `const` by default, `let` for reassignable variables, avoid `var`
- ✅ Use `async/await` instead of `.then()` chains
- ✅ Always validate and sanitize user input
- ✅ Use `textContent` instead of `innerHTML` to prevent XSS attacks
- ✅ Use `fetch()` with error handling

```javascript
// ✅ Good: Safe, modern, readable
async function loadProducts() {
  try {
    const response = await fetch('ProductsList.json');
    if (!response.ok) throw new Error('Failed to load');
    const products = await response.json();
    displayProducts(products);
  } catch (error) {
    console.error('Error:', error);
    showErrorMessage('Failed to load products');
  }
}

// ❌ Bad: Vulnerable to XSS, uses deprecated methods
function loadProducts() {
  fetch('ProductsList.json')
    .then(res => res.json())
    .then(products => {
      document.getElementById('container').innerHTML = 
        '<div>' + products[0].title + '</div>'; // XSS vulnerability!
    });
}
```

#### Function Standards
- Use descriptive function names (verb + noun): `loadProducts()`, `formatCurrency()`, `showNotification()`
- Keep functions small (< 50 lines)
- One responsibility per function (Single Responsibility Principle)
- Add JSDoc comments for complex functions

```javascript
/**
 * Format a number as USD currency
 * @param {number} price - The price to format
 * @returns {string} Formatted currency string (e.g., "$99.99")
 */
function formatCurrency(price) {
  if (typeof price !== 'number' || price < 0) {
    throw new Error('Price must be a non-negative number');
  }
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD'
  }).format(price);
}
```

#### Event Handling
- Use `addEventListener()` instead of inline `onclick` attributes
- Namespace event handlers to avoid conflicts
- Clean up event listeners to prevent memory leaks

```javascript
// ✅ Good: Proper event handling
document.getElementById('addBtn').addEventListener('click', (event) => {
  event.preventDefault();
  addToCart();
});

// ❌ Bad: Inline handler, not accessible
<button onclick="addToCart()">Add</button>
```

#### Data Validation
- Always validate API responses
- Check for null/undefined before accessing properties
- Provide user-friendly error messages

```javascript
// ✅ Good: Validates data
function displayProducts(products) {
  if (!Array.isArray(products) || products.length === 0) {
    showError('No products available');
    return;
  }
  
  products.forEach(product => {
    if (!product.id || !product.title || typeof product.price !== 'number') {
      console.warn('Invalid product:', product);
      return;
    }
    renderProductCard(product);
  });
}
```

#### Formatting Standards
- Use **2-space indentation**
- Variable names: `camelCase` (e.g., `productId`, `isLoading`)
- Constants: `SCREAMING_SNAKE_CASE` (e.g., `MAX_RETRIES`)
- Line length: max 100 characters
- Use semicolons at end of statements

```javascript
const MAX_RETRIES = 3;
const API_TIMEOUT = 5000;

function fetchProductData(productId) {
  const data = getProduct(productId);
  return data;
}
```

---

## 4. Accessibility Guidelines (WCAG 2.1 AA)

### 4.1 Keyboard Navigation
- ✅ All interactive elements focusable via Tab key
- ✅ Focus order logical (left-to-right, top-to-bottom)
- ✅ Skip links for navigation
- ✅ Enter/Space to activate buttons

```html
<!-- Add skip link at top of body -->
<a href="#main-content" class="skip-link">Skip to main content</a>

<main id="main-content">
  <!-- Main content -->
</main>

<style>
  .skip-link {
    position: absolute;
    left: -9999px;
  }
  .skip-link:focus {
    left: 0;
  }
</style>
```

### 4.2 Screen Reader Support
- ✅ Use semantic HTML (`<button>`, `<nav>`, `<main>`)
- ✅ Add `aria-label` for icons-only buttons
- ✅ Use `aria-live` for dynamic content updates
- ✅ Add `role` attribute where semantic elements aren't available

```html
<!-- ✅ Good: Clear for screen readers -->
<button aria-label="Add Wireless Headphones to cart">
  <svg aria-hidden="true">...</svg>
</button>

<!-- ✅ Good: aria-live for notifications -->
<div aria-live="polite" aria-atomic="true" id="notification"></div>

<!-- ❌ Bad: No labels -->
<button>+</button>
```

### 4.3 Color & Contrast
- ✅ Minimum 4.5:1 contrast ratio for text
- ✅ Don't rely on color alone to convey information
- ✅ Use colorblind-friendly palettes

```css
/* ✅ Good: High contrast */
color: #333;           /* Dark text on white background = 14:1 contrast */
background: #ffffff;

/* ❌ Bad: Low contrast */
color: #999;           /* Gray on white = 2.3:1 contrast */
background: #f5f5f5;
```

### 4.4 Mobile Accessibility
- ✅ Touch targets minimum 48px × 48px
- ✅ No small text (min 14px, 16px for body)
- ✅ Sufficient spacing between interactive elements

```css
/* ✅ Good: Accessible touch targets */
.button {
  min-height: 48px;
  min-width: 48px;
  padding: 12px 20px;
}

/* ✅ Good: Readable font size */
body {
  font-size: 16px;
}
```

---

## 5. Security Guidelines

### 5.1 Input Validation & Sanitization
- ✅ Never use `innerHTML` with user input
- ✅ Validate and sanitize all data
- ✅ Use `textContent` for user-generated content

```javascript
// ✅ Good: Safe
function displayProduct(product) {
  const title = document.createElement('h2');
  title.textContent = product.title; // Safe - no HTML parsing
  container.appendChild(title);
}

// ❌ Bad: XSS vulnerability
function displayProduct(product) {
  container.innerHTML = `<h2>${product.title}</h2>`; // Dangerous!
}
```

### 5.2 API Security
- ✅ Use HTTPS only
- ✅ Validate CORS headers
- ✅ Use content security policy (CSP)
- ✅ Don't expose sensitive data in client-side code

```html
<!-- Add CSP header -->
<meta http-equiv="Content-Security-Policy" 
      content="default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'">
```

### 5.3 Error Handling
- ✅ Don't expose server errors to users
- ✅ Log errors securely (never log passwords or tokens)
- ✅ Provide user-friendly error messages

```javascript
// ✅ Good: Secure error handling
try {
  const response = await fetch('/api/products');
  if (!response.ok) throw new Error('API request failed');
  return await response.json();
} catch (error) {
  console.error('Error loading products'); // Generic message
  showUserError('Unable to load products. Please try again.');
}
```

---

## 6. Testing Guidelines

### 6.1 Unit Testing
- Test individual functions in isolation
- Aim for 80%+ code coverage
- Use a framework like Jest, Vitest, or Mocha

```javascript
// Example test with Jest
describe('formatCurrency', () => {
  test('formats number as USD', () => {
    expect(formatCurrency(99.99)).toBe('$99.99');
  });
  
  test('throws error for invalid input', () => {
    expect(() => formatCurrency('invalid')).toThrow();
  });
});
```

### 6.2 Integration Testing
- Test component interactions
- Test data fetching and rendering
- Test error states

```javascript
// Example integration test
describe('Product Page', () => {
  test('loads and displays products from JSON', async () => {
    render(<ProductPage />);
    await waitFor(() => {
      expect(screen.getByText('Wireless Headphones')).toBeInTheDocument();
    });
  });
});
```

### 6.3 Accessibility Testing
- ✅ Use Axe DevTools or WAVE browser extensions
- ✅ Test with keyboard navigation only
- ✅ Test with screen readers (NVDA, JAWS, VoiceOver)
- ✅ Check color contrast with WebAIM Contrast Checker

**Manual Testing Checklist:**
- [ ] Tab through all elements - all interactive items reachable
- [ ] Screen reader reads content correctly
- [ ] Focus indicators visible on all interactive elements
- [ ] No color-only information conveyance
- [ ] Touch targets ≥ 48px × 48px

### 6.4 Performance Testing
- ✅ Lighthouse audit (target > 90)
- ✅ WebPageTest for real-world performance
- ✅ PageSpeed Insights for Core Web Vitals
- ✅ Load testing under slow networks

**Performance Targets:**
- Largest Contentful Paint (LCP): < 2.5s
- First Input Delay (FID): < 100ms
- Cumulative Layout Shift (CLS): < 0.1

### 6.5 Cross-Browser Testing
Test on:
- Chrome/Edge (latest 2 versions)
- Firefox (latest 2 versions)
- Safari (latest 2 versions)
- Mobile browsers (iOS Safari, Chrome Mobile)

---

## 7. Performance Guidelines

### 7.1 Asset Optimization
- **Images**: Use optimized formats (WebP with fallback), compress, provide multiple sizes
  ```html
  <picture>
    <source srcset="product.webp" type="image/webp">
    <img src="product.jpg" alt="Product" loading="lazy">
  </picture>
  ```

- **CSS**: Inline critical styles, defer non-critical CSS, minify
- **JavaScript**: Lazy load scripts, code splitting, minify

### 7.2 Network Optimization
- **Caching**: Use browser caching with proper headers
- **CDN**: Serve assets from geographically distributed servers
- **Compression**: Enable gzip/brotli compression
- **HTTP/2**: Use HTTP/2 protocol support

### 7.3 Rendering Optimization
- ✅ Avoid layout thrashing (batch DOM reads/writes)
- ✅ Use `requestAnimationFrame` for animations
- ✅ Debounce scroll/resize events
- ✅ Use `will-change` CSS for expensive animations (sparingly)

```javascript
// ✅ Good: Batch DOM operations
function addProducts(products) {
  const fragment = document.createDocumentFragment();
  products.forEach(product => {
    fragment.appendChild(createProductElement(product));
  });
  container.appendChild(fragment); // Single reflow
}

// ❌ Bad: Multiple reflows
function addProducts(products) {
  products.forEach(product => {
    container.appendChild(createProductElement(product)); // Triggers reflow each time
  });
}
```

### 7.4 Bundle Size
- Target < 50KB (uncompressed HTML + CSS + JS)
- Monitor with bundlesize or similar tools
- Use tree-shaking to eliminate dead code

---

## 8. Code Formatting Standards

### 8.1 Consistent Style
- **Formatter**: Use Prettier for automatic formatting
- **Linter**: Use ESLint for JavaScript, StyleLint for CSS
- **Pre-commit hooks**: Use Husky to enforce standards

```json
{
  ".prettierrc": {
    "semi": true,
    "singleQuote": true,
    "trailingComma": "es5",
    "tabWidth": 2,
    "printWidth": 100
  }
}
```

### 8.2 File Organization
```
project/
├── index.html
├── css/
│   └── styles.css
├── js/
│   ├── main.js
│   ├── utils.js
│   └── api.js
├── data/
│   └── ProductsList.json
├── tests/
│   ├── main.test.js
│   └── utils.test.js
└── README.md
```

### 8.3 Naming Conventions
| Type | Convention | Example |
|------|-----------|---------|
| CSS Classes | kebab-case | `.product-card`, `.add-to-cart-btn` |
| CSS IDs | kebab-case | `#main-content`, `#modal-dialog` |
| JavaScript Variables | camelCase | `productId`, `isLoading` |
| JavaScript Constants | SCREAMING_SNAKE_CASE | `MAX_RETRIES`, `API_URL` |
| JavaScript Functions | camelCase (verb first) | `fetchProducts()`, `renderCard()` |

---

## 9. Documentation Standards

### 9.1 Code Comments
- Only comment "why", not "what" (code should be self-documenting)
- Use JSDoc for functions and complex logic
- Keep comments up-to-date with code changes

```javascript
// ❌ Bad: Comments state the obvious
const price = 99.99; // Set price to 99.99

// ✅ Good: Comments explain the reasoning
// Pricing includes 15% seasonal discount (expires 2026-06-01)
const price = 99.99;

/**
 * Fetches product data from the API with retry logic
 * @async
 * @param {string} productId - The ID of the product
 * @param {number} retries - Number of retry attempts (default: 3)
 * @returns {Promise<Object>} Product data object
 * @throws {Error} If all retries fail
 */
async function fetchProductWithRetry(productId, retries = 3) {
  // Implementation
}
```

### 9.2 README Documentation
Include:
- Project overview
- Installation instructions
- Usage examples
- Accessibility features
- Security practices
- Performance metrics
- Browser support
- Contributing guidelines

---

## 10. Deployment Checklist

- [ ] All code passes linting (ESLint, StyleLint)
- [ ] All tests passing (unit, integration, accessibility)
- [ ] Lighthouse score ≥ 90
- [ ] WCAG 2.1 AA compliance verified
- [ ] Cross-browser testing completed
- [ ] Security audit passed
- [ ] Performance budget met (< 50KB)
- [ ] CSP headers configured
- [ ] HTTPS enabled
- [ ] Error monitoring configured (Sentry, etc.)
- [ ] Analytics configured (GA4, etc.)
- [ ] README documentation updated

---

## 11. Tools & Technologies

### Required Tools
- **Code Editor**: VS Code
- **Formatter**: Prettier
- **Linter**: ESLint, StyleLint
- **Testing**: Jest, Testing Library
- **Accessibility**: Axe DevTools, WAVE
- **Performance**: Lighthouse, WebPageTest
- **Version Control**: Git, GitHub

### Browser Extensions (Development)
- Axe DevTools (Accessibility)
- WAVE (Accessibility)
- Lighthouse (Performance)
- WebAIM Contrast Checker (Color Contrast)
- Visual Inspector (CSS Debugging)

---

## 12. Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-05-04 | Initial PRD creation |

---

## 13. Appendix: Quick Reference

### HTML Best Practices
```html
<!-- Always include -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Page Title</title>
</head>
<body>
  <!-- Semantic structure -->
  <header>Header</header>
  <main>Main content</main>
  <footer>Footer</footer>
</body>
</html>
```

### CSS Best Practices
```css
/* Mobile-first approach */
.element {
  font-size: 16px;
  padding: 16px;
}

/* Tablet and up */
@media (min-width: 768px) {
  .element {
    font-size: 18px;
    padding: 20px;
  }
}

/* Desktop and up */
@media (min-width: 1024px) {
  .element {
    font-size: 20px;
    padding: 24px;
  }
}
```

### JavaScript Best Practices
```javascript
// Use async/await for cleaner code
async function loadData() {
  try {
    const response = await fetch('/api/data');
    if (!response.ok) throw new Error('Network error');
    return await response.json();
  } catch (error) {
    console.error('Error:', error.message);
    throw error;
  }
}

// Always validate input
function processInput(value) {
  if (!value || typeof value !== 'string') {
    throw new Error('Invalid input');
  }
  return value.trim().toLowerCase();
}
```

---

**Document Owner**: Development Team  
**Last Reviewed**: 2026-05-04  
**Next Review**: 2026-08-04

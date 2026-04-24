# Customization Guide

This guide walks you through common customizations to the AI Cost Calculator.

## 1. Change Base Year Costs

The calculator starts with default costs based on typical enterprise deployments. Adjust these to match your client profiles.

### Locate the code:

Open `index.html` and find this section (search for "baseYearCosts"):

```javascript
const baseYearCosts = {
    'Infrastructure & Compute': { base: 250000, factor: assumptions.deploymentScale / 100 },
    'Software & Licensing': { base: 150000, factor: assumptions.deploymentScale / 100 },
    'Data Management': { base: 120000, factor: (assumptions.dataReadiness / 100) * (assumptions.deploymentScale / 100) },
    'Talent & Training': { base: 350000, factor: (assumptions.staffingLevel === 'high' ? 1.3 : assumptions.staffingLevel === 'medium' ? 1 : 0.7) },
    'Integration & Development': { base: 200000, factor: assumptions.deploymentScale / 100 },
    'Governance & Security': { base: 100000, factor: 1 }
};
```

### Modify values:

For example, if your typical client has lower infrastructure costs:

```javascript
const baseYearCosts = {
    'Infrastructure & Compute': { base: 180000, factor: assumptions.deploymentScale / 100 },  // Changed from 250k
    'Software & Licensing': { base: 120000, factor: assumptions.deploymentScale / 100 },      // Changed from 150k
    // ... rest unchanged
};
```

## 2. Change Default Growth Rates

Growth rates default to industry averages, but you may want different values for your specific market or client profile.

### Locate the code:

Find the `useState` hook (search for "infrastructureGrowthRate"):

```javascript
const [assumptions, setAssumptions] = useState({
    deploymentScale: 100,
    dataReadiness: 70,
    staffingLevel: 'medium',
    infrastructureGrowthRate: 0.08,        // 8% default
    licenseGrowthRate: 0.12,               // 12% default
    dataGrowthRate: 0.10,                  // 10% default
    talentGrowthRate: 0.05,                // 5% default
    integrationGrowthRate: 0.06,           // 6% default
    governanceGrowthRate: 0.04             // 4% default
});
```

### Modify values:

For a market where licensing costs grow faster:

```javascript
licenseGrowthRate: 0.15,  // Changed from 0.12 (12% to 15%)
```

## 3. Add a New Cost Category

Want to add "Change Management" or "Quality Assurance"? Here's how:

### Step 1: Add to baseYearCosts

```javascript
const baseYearCosts = {
    // ... existing categories ...
    'Change Management': { base: 80000, factor: assumptions.deploymentScale / 100 },
};
```

### Step 2: Add to growthRates

```javascript
const growthRates = {
    // ... existing rates ...
    'Change Management': assumptions.changeManagementGrowthRate,
};
```

### Step 3: Add to useState

```javascript
const [assumptions, setAssumptions] = useState({
    // ... existing assumptions ...
    changeManagementGrowthRate: 0.07,
});
```

### Step 4: Add to growth rates grid (in JSX)

Find the growth rates section and add:

```jsx
{ key: 'changeManagementGrowthRate', label: 'Change Management' },
```

## 4. Change the Slider Ranges

Currently, deployment scale goes from 25% to 200%. To change:

### Locate the sliders in the JSX:

```jsx
<input type="range" min="25" max="200" value={assumptions.deploymentScale} ... />
```

### Modify the min/max values:

```jsx
<input type="range" min="10" max="250" value={assumptions.deploymentScale} ... />
```

Do the same for other sliders (data readiness, growth rates, etc.).

## 5. Adjust Staffing Level Multipliers

Currently: Low = 0.7x, Medium = 1.0x, High = 1.3x

### Locate the code:

```javascript
'Talent & Training': { base: 350000, factor: (assumptions.staffingLevel === 'high' ? 1.3 : assumptions.staffingLevel === 'medium' ? 1 : 0.7) },
```

### Modify the multipliers:

```javascript
'Talent & Training': { base: 350000, factor: (assumptions.staffingLevel === 'high' ? 1.5 : assumptions.staffingLevel === 'medium' ? 1 : 0.5) },
```

## 6. Change Default Assumptions

The calculator starts with these defaults:
- Deployment scale: 100%
- Data readiness: 70%
- Staffing level: medium

### Locate and modify:

```javascript
const [assumptions, setAssumptions] = useState({
    deploymentScale: 100,      // Change this
    dataReadiness: 70,         // Or this
    staffingLevel: 'medium',   // Or this
    // ...
});
```

## 7. Customize the Descriptions (Help Text)

The calculator shows hints like "Full deployment at 100%, pilot at 50%"

### Locate in JSX:

```jsx
<small>Full deployment at 100%, pilot at 50%</small>
```

### Customize for your clients:

```jsx
<small>Adjust for your scope: 50% = division rollout, 100% = enterprise-wide</small>
```

## 8. Change Colors & Styling

All styling is in the `<style>` block at the top of `index.html`

### Accent color (currently blue):

```css
accent-color: #0066cc;  /* Change to your brand color */
border-color: #0088dd;  /* Related notes box accent */
```

### Background colors for dark mode:

```css
@media (prefers-color-scheme: dark) {
    body { background: #1a1a1a; }  /* Adjust darkness */
    /* etc */
}
```

## 9. Remove or Hide Cost Categories

If you don't want to display "Governance & Security", comment out:

1. Remove from `baseYearCosts`
2. Remove from `growthRates`
3. Remove from the growth rates grid

The table will automatically update.

## 10. Change Currency or Number Format

### Locate the format function:

```javascript
const formatCurrency = (num) => {
    return new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD', maximumFractionDigits: 0 }).format(num);
};
```

### Change for different currency:

```javascript
currency: 'EUR'  // Euro
currency: 'GBP'  // British Pound
currency: 'CAD'  // Canadian Dollar
```

Or to show currency in thousands (e.g., "$250k" instead of "$250,000"):

```javascript
const formatCurrency = (num) => {
    return new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD', maximumFractionDigits: 0 }).format(num / 1000) + 'k';
};
```

## Testing Your Changes

1. **Open in browser**: Double-click `index.html` or use a local server
2. **Test sliders**: Move them to min/max and check calculations
3. **Verify totals**: Run quick math to spot errors
4. **Check display**: Ensure new categories appear in table and metrics

## Common Mistakes to Avoid

- ❌ Forgetting to add new assumptions to `useState` (will cause undefined errors)
- ❌ Typos in category names (must match across `baseYearCosts`, `growthRates`, and JSX)
- ❌ Not updating slider labels when you change min/max values
- ❌ Removing closing braces or semicolons (breaks the whole app)
- ❌ Hardcoding costs instead of using `assumptions` (static values won't update when user adjusts sliders)

## Troubleshooting

**Calculator won't load?**
- Check browser console for JavaScript errors (F12 → Console)
- Verify you didn't break any braces `{}` or parentheses `()`

**Numbers seem wrong?**
- Verify base year costs in `baseYearCosts`
- Check growth rates are in decimal form (0.08 = 8%, not 8)
- Ensure formulas reference correct assumptions

**A slider doesn't work?**
- Check that the corresponding assumption in `useState` matches the key name exactly
- Verify the `onChange` handler references the right assumption key

Need help? Open an issue on GitHub or contact your team.

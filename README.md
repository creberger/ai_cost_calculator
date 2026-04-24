# AI Implementation Cost Calculator

An interactive web-based cost modeling tool for planning AI implementation budgets across a 5-year period.

## Features

- **Interactive assumptions**: Adjust deployment scale, data readiness, staffing levels, and growth rates in real-time
- **6 cost categories**: Infrastructure, Software, Data Management, Talent, Integration, and Governance
- **5-year projections**: Year-by-year breakdown with automated growth calculations
- **Professional layout**: Clean, responsive design suitable for client presentations
- **Dark mode support**: Automatically adapts to system preferences
- **No build required**: Runs directly in the browser via CDN

## Quick Start

### Option 1: GitHub Pages (Easiest)

1. **Fork or clone this repository**
   ```bash
   git clone https://github.com/yourusername/ai-cost-calculator.git
   cd ai-cost-calculator
   ```

2. **Enable GitHub Pages**
   - Go to Settings → Pages
   - Select "Deploy from a branch"
   - Choose `main` branch and `/root` folder
   - Your calculator will be live at `https://yourusername.github.io/ai-cost-calculator/`

3. **That's it!** Share the link with clients. No build step needed.

### Option 2: Local Development

Just open `index.html` in your browser—no dependencies required!

For a local server:
```bash
# Using Python 3
python -m http.server 8000

# Or using Node's http-server
npx http-server
```

Then visit `http://localhost:8000`

### Option 3: Deploy Elsewhere

The single `index.html` file works anywhere:
- **Netlify**: Drag and drop `index.html`
- **Vercel**: Push to a repo, Vercel auto-deploys
- **AWS S3 + CloudFront**: Upload and configure as static website
- **Your own server**: Copy `index.html` to any web server

## File Structure

```
├── index.html              # Single self-contained calculator
├── README.md               # This file
├── package.json            # Project metadata
└── docs/
    └── CUSTOMIZATION.md    # How to modify costs & assumptions
```

## How to Use

1. **Adjust deployment parameters** on the left side:
   - Scale: 50% (pilot) to 200% (enterprise)
   - Data readiness: 30% to 100%
   - Staffing level: Low, Medium, or High

2. **Modify growth rates** for each cost category (defaults are conservative estimates)

3. **View real-time projections**:
   - Metric cards show Year 1, Year 5, 5-year total, and average annual costs
   - Detailed table shows breakdown by category and year

4. **Share or present**:
   - Simply share the URL with stakeholders
   - They can modify assumptions without affecting your version
   - No login or sign-up required

## Customizing the Model

### Modify base year costs

Edit the `baseYearCosts` object in the `<script>` section:

```javascript
const baseYearCosts = {
    'Infrastructure & Compute': { base: 250000, factor: ... },
    'Software & Licensing': { base: 150000, factor: ... },
    // ... update base values for your typical customer
};
```

### Change growth rate defaults

Edit the `useState` initial values:

```javascript
const [assumptions, setAssumptions] = useState({
    infrastructureGrowthRate: 0.08,  // Change default from 8% to your preference
    licenseGrowthRate: 0.12,         // e.g., 0.10 for 10%
    // ...
});
```

### Add or remove cost categories

1. Add to `baseYearCosts` object
2. Add to `growthRates` object
3. Add to the growth rates grid in the JSX

See `docs/CUSTOMIZATION.md` for detailed examples.

## Cost Categories Explained

| Category | Includes | Scaling |
|----------|----------|---------|
| **Infrastructure & Compute** | Cloud compute, GPUs, storage, networking | Scales with deployment size |
| **Software & Licensing** | ML platforms, tools, APIs, frameworks | Scales with deployment size |
| **Data Management** | ETL tools, data warehouses, quality, governance | Scales with deployment size × data readiness |
| **Talent & Training** | Hiring, salaries, external consultants, training | Scales with staffing level |
| **Integration & Development** | Custom development, APIs, system integration | Scales with deployment size |
| **Governance & Security** | Compliance, monitoring, security, auditing | Fixed + minimal scaling |

## Assumptions & Caveats

This calculator provides **estimates based on industry benchmarks**:

- ✅ Good for exploratory budget planning
- ✅ Useful for comparing scenarios
- ❌ Not a substitute for vendor quotes
- ❌ Does not account for org-specific complexity
- ❌ Growth rates are averages; your project may vary

**Always validate with actual vendor quotes before finalizing budgets.**

## Browser Support

- ✅ Chrome/Edge 88+
- ✅ Firefox 78+
- ✅ Safari 14+
- ✅ Mobile browsers

## Technology Stack

- **React** 18 (via CDN)
- **Babel** (for JSX transpilation)
- **Vanilla CSS** (no build tools needed)
- **No backend** (everything runs client-side)

## Contributing

Found a bug? Want to improve the model?

1. Fork the repo
2. Create a branch: `git checkout -b improve/better-defaults`
3. Commit your changes: `git commit -am 'Improve growth rate logic'`
4. Push and open a PR

## License

MIT — Use freely in your consulting work. Attribution appreciated but not required.

## Contact & Support

Questions about the calculator or want to customize it for your clients?

- 📧 Email: your-email@example.com
- 🐙 GitHub Issues: Use the Issues tab for bugs
- 📝 Discussions: Use Discussions for feature ideas

---

**Last updated**: April 2026  
**Version**: 1.0  
**Maintained by**: [44consulting](https://44consulting.net)

# Next.js + Tailwind Accessibility Audit

AI-assisted accessibility auditing for Next.js applications using Tailwind CSS. This repository provides a comprehensive audit framework designed specifically for LLM consumption to identify, prioritise, and fix accessibility issues systematically.

## Quick Start

### 1. Upload Your Project

Share your Next.js project files with an LLM along with the `audit.xml` document.

### 2. Run the Audit

The `audit.xml` contains everything needed to perform a comprehensive accessibility audit. Simply upload both your codebase and the XML file - no additional prompts required.

The LLM will automatically:

- Apply the structured audit methodology
- Present each violation individually for review
- Provide specific WCAG 2.2 compliance guidance
- Output structured JSON for implementation

### 3. Review Violations and Fixes

The LLM will present each accessibility issue one at a time with suggested fixes. You can review, verify the LLM has interpreted the codebase correctly, and approve or modify the proposed solutions.

### 4. Process Results

At the end, the LLM will output structured JSON listing each issue that can be fed directly into implementation agents or development workflows.

### 5. Implementation Agent

Feed a prompt like this into your implementation agent (recommend to use [OpenAI Codex](https://openai.com/index/introducing-codex/)):

```markdown
Apply required changes listed in JSON accessibility audit below:
[PASTE COMPLETE JSON OUTPUT]

Work through each fix systematically, maintaining code quality and testing.
```

## What Gets Audited

### Core WCAG Compliance (21 Areas)

- **Page Structure**: Unique titles, heading hierarchy, layout consistency
- **Visual Design**: Colour contrast (4.5:1 ratio), touch targets (24x24px minimum)
- **Content**: Alt text, skip links, semantic HTML optimisation
- **Interaction**: Focus management, ARIA implementation, form accessibility
- **Media**: Video captions, motion preferences, auto-playing content
- **Dynamic Content**: Live regions, language support, time limits

### WCAG 2.2 Requirements (4 Areas)

- Focus visibility (not obscured)
- Redundant entry prevention
- Accessible authentication
- Consistent help placement

### Next.js Specific (2 Areas)

- SSR/hydration accessibility
- Client-side routing focus management

### Tailwind Specific (2 Areas)

- Dynamic class accessibility
- Dark mode contrast compliance

## Audit Output Format

Each issue returns structured JSON:

```json
{
  "issue": "unique-id",
  "wcag": ["2.4.7"],
  "severity": "Critical|Serious|Moderate|Minor",
  "location": "components/Button.tsx:42",
  "description": "Missing focus indicator",
  "fix": {
    "before": "<button className=\"bg-blue-500\">",
    "after": "<button className=\"bg-blue-500 focus-visible:ring-2\">",
    "effort": "Low"
  }
}
```

## Implementation Workflow

### 1. Receive Audit Results

Collect all JSON issue objects from the LLM audit.

### 2. Prioritise Fixes

Issues are automatically scored using:

```
Score = Severity × Frequency × User Impact × Legal Risk
```

**Fix Priority:**

- **Immediate**: Blockers + high legal risk
- **Sprint**: Critical issues on key pages
- **Backlog**: Moderate issues
- **Nice-to-have**: Minor enhancements

### 3. Implement Fixes

Use the audit output to instruct implementation agents:

```
Apply this accessibility fix:
- Issue: Missing focus indicator
- Location: components/Button.tsx:42
- WCAG: 2.4.7
- Change: "<button className=\"bg-blue-500\">" to "<button className=\"bg-blue-500 focus-visible:ring-2\">"
- Effort: Low

Test the fix with keyboard navigation to ensure focus is visible.
```

### 4. Verify Implementation

Run automated tools for verification:

- **axe-core**: Covers ~57% of WCAG issues
- **Pa11y**: CLI-friendly, CI/CD integration
- **Lighthouse**: Quick assessment
- **Manual testing**: Required for remaining ~43%

## Quick Wins Identified

The audit automatically flags these common, high-impact issues:

1. Missing unique page titles
2. Multiple h1s or skipped heading levels
3. Pages without main landmark
4. Colour contrast failures
5. Missing/inappropriate alt text
6. Non-functional skip links
7. Touch targets below 24x24px
8. Videos without captions
9. Deep div nesting vs semantic HTML
10. Dynamic content without live regions
11. Missing motion-reduce alternatives
12. Forms without proper labelling
13. Focus traps missing from modals
14. Hydration mismatches affecting accessibility
15. Missing route change focus management

## Coverage & Limitations

**Automated Detection**: ~57% of WCAG issues via tooling
**Manual Testing Required**: ~43% of issues need human verification
**False Positives**: Near-zero when using structured approach

**Not Covered:**

- Cognitive accessibility nuances
- Content readability assessment
- Complex user journey testing
- Screen reader specific testing

## Integration Options

### CI/CD Pipeline

```yaml
- name: Accessibility Audit
  run: |
    # Upload codebase + audit.xml to LLM
    # Process JSON output
    # Fail build on Critical/Blocker issues
```

### Development Workflow

1. Feature branch creation
2. LLM audit on changed components
3. Fix implementation via agents
4. Automated verification
5. Manual testing for uncovered areas

## Best Practices

- **Component-Level**: Audit individual components for reusable fixes
- **Systematic Patterns**: Focus on architectural improvements over one-off fixes
- **Documentation**: Maintain accessibility patterns in your design system
- **Regular Audits**: Run on feature branches, not just releases
- **Team Training**: Use audit results to educate developers on common patterns

---

This framework transforms accessibility auditing from manual, time-intensive work into systematic, AI-assisted quality assurance that scales with your development process.

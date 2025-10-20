# Skill Design Patterns

Best practices and proven patterns for designing effective Skills.

## Fundamental Principles

### 1. Conciseness Over Comprehensiveness
- Default assumption: Claude is already smart
- Only add context Claude doesn't have
- Challenge each piece: "Does Claude really need this?"
- Prefer examples over verbose explanations

### 2. Appropriate Freedom Levels

**High Freedom (text instructions):**
- Multiple valid approaches
- Context-dependent decisions
- Heuristics guide the process
- Example: Content creation, analysis, brainstorming

**Medium Freedom (pseudocode/parameters):**
- Preferred pattern exists
- Some variation acceptable
- Configuration affects behavior
- Example: Data formatting, template application

**Low Freedom (specific scripts):**
- Operations are fragile/error-prone
- Consistency is critical
- Specific sequence required
- Example: PDF manipulation, file parsing

### 3. Progressive Disclosure

**Three-level loading:**
1. Metadata (always in context) - ~100 words
2. SKILL.md body (when triggered) - <5k words
3. Bundled resources (as needed) - Unlimited

**Keep SKILL.md lean:**
- Core workflows only
- Reference external files for details
- Under 500 lines preferred

## Structural Patterns

### Pattern 1: Workflow-Based
Best for sequential processes.

```markdown
## Overview
Brief description

## Workflow
Step-by-step process with decision points

## Step 1: [Action]
Detailed instructions

## Step 2: [Action]
Detailed instructions
```

**Example skills:** Document editing, data processing

### Pattern 2: Task-Based
Best for tool collections.

```markdown
## Overview
What this skill provides

## Quick Start
Most common use case

## Task Category 1
Related operations

## Task Category 2
Related operations
```

**Example skills:** PDF tools, spreadsheet operations

### Pattern 3: Capabilities-Based
Best for integrated systems.

```markdown
## Overview
System description

## Core Capabilities
### 1. Feature Name
What it does and when to use

### 2. Feature Name
What it does and when to use
```

**Example skills:** Analysis tools, content generators

### Pattern 4: Reference/Guidelines
Best for standards and specifications.

```markdown
## Overview
Purpose of guidelines

## Guidelines
Core principles

## Specifications
Detailed requirements

## Usage
Application examples
```

**Example skills:** Brand guidelines, coding standards

## Output Quality Patterns

### Template Pattern (Strict)
For consistent, predictable output.

```markdown
## Output Format

ALWAYS use this exact structure:

# [Title]

## Section 1
[Required content type]

## Section 2
[Required content type]
```

**Use when:** API responses, reports, data formats

### Template Pattern (Flexible)
For adaptable guidance.

```markdown
## Output Format

Here's a sensible default, adjust as needed:

# [Title]

## Section 1
[Suggested content]

Adapt sections based on context.
```

**Use when:** Creative content, analysis, recommendations

### Examples Pattern
For quality through demonstration.

```markdown
## Output Style

Follow these examples:

**Example 1:**
Input: [scenario]
Output: [desired result]

**Example 2:**
Input: [scenario]
Output: [desired result]
```

**Use when:** Writing style, formatting, transformations

## Resource Organization Patterns

### Pattern: Domain Separation
Split references by domain to avoid loading irrelevant context.

```
skill-name/
├── SKILL.md (overview + navigation)
└── references/
    ├── domain1.md
    ├── domain2.md
    └── domain3.md
```

**SKILL.md should reference:**
"For domain1 operations, see references/domain1.md"

**Use when:** Skill covers multiple distinct areas

### Pattern: Complexity Layers
Progressive detail for different user needs.

```
skill-name/
├── SKILL.md (quick start + essentials)
└── references/
    ├── QUICK_REFERENCE.md (common patterns)
    ├── ADVANCED.md (edge cases)
    └── API_REFERENCE.md (complete details)
```

**Use when:** Skills with beginner and advanced users

### Pattern: Asset Templates
Reusable starting points.

```
skill-name/
├── SKILL.md
└── assets/
    ├── template1/
    ├── template2/
    └── boilerplate/
```

**SKILL.md should guide selection:**
"For [use case], start with assets/template1/"

**Use when:** Multiple starting point options

## Workflow Design Patterns

### Sequential Workflow
Clear step-by-step processes.

```markdown
This task involves:

1. Preparation (run prepare.py)
2. Processing (run process.py)
3. Validation (run validate.py)
4. Output (run generate.py)
```

**Use when:** Order matters, steps depend on previous steps

### Conditional Workflow
Branching logic based on context.

```markdown
1. Determine the task type:
   **Type A?** → Follow workflow A below
   **Type B?** → Follow workflow B below

## Workflow A
[Steps for type A]

## Workflow B
[Steps for type B]
```

**Use when:** Different approaches for different scenarios

### Parallel Workflow
Independent operations that can happen in any order.

```markdown
Complete these operations (order doesn't matter):

- Operation 1: [instructions]
- Operation 2: [instructions]
- Operation 3: [instructions]

Then combine results: [final step]
```

**Use when:** Independent tasks that converge

## Script Patterns

### When to Include Scripts

**✅ Include scripts when:**
- Same code rewritten repeatedly
- Operations are fragile/error-prone
- Deterministic reliability needed
- Complex algorithms required
- File format manipulation

**❌ Don't include scripts when:**
- Simple transformations
- High variability in approach
- Context-dependent logic
- Claude can write it easily

### Script Organization

**Single-purpose scripts:**
```
scripts/
├── rotate_pdf.py
├── extract_text.py
└── merge_pdfs.py
```

**Module-based scripts:**
```
scripts/
├── __init__.py
├── core.py
├── utilities.py
└── validators.py
```

**Script + config pattern:**
```
scripts/
├── process.py
└── config/
    ├── default.json
    └── advanced.json
```

## Common Anti-Patterns

### ❌ Over-Specification
Providing too much detail for simple tasks.

**Bad:**
```markdown
To format text as bold:
1. Locate the text
2. Wrap in markdown bold syntax (**)
3. Ensure no extra spaces
4. Validate formatting
```

**Good:**
```markdown
Format important text as **bold**.
```

### ❌ Under-Specification for Fragile Tasks
Too much freedom for error-prone operations.

**Bad:**
```markdown
Fill the PDF form fields with the data.
```

**Good:**
```markdown
Fill PDF form fields:
1. Run scripts/analyze_form.py to identify fields
2. Map data to field names in mapping.json
3. Run scripts/fill_form.py with mapping
4. Verify output with scripts/validate.py
```

### ❌ Hidden Resources
Not referencing external files from SKILL.md.

**Bad:**
SKILL.md doesn't mention references/advanced.md

**Good:**
```markdown
For advanced features, see references/advanced.md
```

### ❌ Duplicated Content
Same information in multiple places.

**Bad:**
- SKILL.md contains full API documentation
- references/api.md contains same information

**Good:**
- SKILL.md: "For API details, see references/api.md"
- references/api.md: Contains full documentation

### ❌ Auxiliary Documentation
Creating files Claude doesn't need.

**Bad:**
```
skill-name/
├── SKILL.md
├── README.md
├── INSTALLATION.md
└── CHANGELOG.md
```

**Good:**
```
skill-name/
└── SKILL.md
```

## Context Management Strategies

### Strategy: Conditional Loading
Guide when to load references.

```markdown
## Using This Skill

**For basic operations:** Use instructions below
**For advanced features:** Load references/advanced.md
**For API integration:** Load references/api.md
```

### Strategy: Grep Patterns
Help Claude find information quickly in large files.

```markdown
## Large Reference Files

references/schemas.md is 15k words. To find relevant sections:

- `grep "user_table" references/schemas.md` for user data
- `grep "analytics_" references/schemas.md` for analytics tables
```

### Strategy: Table of Contents
Structure preview for long references.

```markdown
# Advanced Features (120 lines)

## Table of Contents
1. Feature A (lines 10-30)
2. Feature B (lines 31-60)
3. Feature C (lines 61-90)
4. Edge Cases (lines 91-120)
```

## Quality Checklist

Before finalizing a Skill, verify:

**Structure:**
- [ ] YAML frontmatter complete (name, description)
- [ ] Description includes when to use the skill
- [ ] SKILL.md under 500 lines (or split appropriately)
- [ ] All references mentioned in SKILL.md
- [ ] No auxiliary documentation (README, etc.)

**Content:**
- [ ] Concise - only non-obvious information
- [ ] Appropriate freedom level for task fragility
- [ ] Examples where helpful
- [ ] Clear workflow or organization
- [ ] Proper progressive disclosure

**Resources:**
- [ ] Scripts tested and working
- [ ] References organized by domain/complexity
- [ ] Assets properly structured
- [ ] No unused example files

**Usability:**
- [ ] Clear trigger scenarios in description
- [ ] Obvious entry point in SKILL.md
- [ ] Guidance for when to load references
- [ ] Concrete usage examples

## Iteration Patterns

### Pattern: Usage-Driven Refinement

1. **Deploy and observe** - Use skill on real tasks
2. **Notice friction** - Where does it struggle?
3. **Identify cause** - Missing info? Wrong freedom level?
4. **Targeted fix** - Update specific section
5. **Test again** - Verify improvement

### Pattern: Complexity Evolution

**Start simple:**
```markdown
# Quick Start

Basic workflow for common case.
```

**Add depth as needed:**
```markdown
# Quick Start

Basic workflow.

For variations, see references/patterns.md
```

### Pattern: Community Feedback Integration

1. **Share skill** - Get real-world usage
2. **Collect pain points** - What's confusing?
3. **Aggregate patterns** - Common issues?
4. **Systematic improvement** - Address root causes
5. **Document learnings** - Update design patterns

## Skill Naming Conventions

**Good names:**
- Descriptive: "pdf-form-filler" not "pdf-tool"
- Action-oriented: "email-thread-summarizer" not "email-skill"
- Specific: "brand-style-applier" not "styling"
- Kebab-case: "meeting-notes-structurer"

**Avoid:**
- Generic names: "helper", "tool", "utility"
- Redundant suffixes: "skill" is implied
- Unclear scope: "document-processor" (which documents?)
- Abbreviations: "mtg-proc" → "meeting-processor"

## Description Best Practices

**Strong descriptions:**
- Explain what AND when to use
- Include key terms for matching
- Be specific about triggers
- Provide enough detail for selection among 100+ skills

**Example - Bad:**
```yaml
description: Helps with documents
```

**Example - Good:**
```yaml
description: Create, edit, and analyze Word documents (.docx) with tracked changes, comments, and formatting preservation. Use for professional document creation, content editing, or when working with .docx files.
```

## Testing Strategies

### Validation Testing
Run the packaging script to check structure:
```bash
scripts/package_skill.py path/to/skill
```

### Functional Testing
Test the skill on real examples:
1. Create new chat
2. Trigger skill naturally
3. Observe behavior
4. Note any confusion or errors
5. Iterate

### Edge Case Testing
Try unusual inputs:
- Missing information
- Ambiguous requests
- Conflicting requirements
- Unusual file formats

### Integration Testing
Test with other skills:
- Do skills conflict?
- Can they work together?
- Is selection logic clear?

## Documentation Principles

### Write for Claude, Not Humans

**Human documentation:**
"This skill is really useful for managing your brand across different platforms and ensuring consistency."

**Claude documentation:**
```markdown
## Brand Style Application

Apply these color codes to all visual content:
- Primary: #141413
- Accent: #d97757
```

### Imperative Form

**Use:** "Run the script", "Load the reference", "Apply the pattern"
**Not:** "You should run", "Consider loading", "It might help to"

### Concrete Examples

**Prefer:**
```markdown
Example usage: "Rotate this PDF 90 degrees clockwise"
→ Run: scripts/rotate.py input.pdf 90 output.pdf
```

**Over:**
```markdown
Use the rotation script to rotate PDFs as needed.
```

### Clear Prerequisites

**State clearly:**
```markdown
## Prerequisites

- Python 3.8+
- pdfplumber library (install: pip install pdfplumber)
- Input PDFs must be unlocked
```

**Not:**
```markdown
Make sure you have the right setup.
```

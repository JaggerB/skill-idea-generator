---
name: skill-idea-generator
description: Generates ideas for new Claude Skills by analyzing workflow patterns, suggesting random ideas by category, or examining conversation history to identify repetitive tasks that could be automated. Use when users want Skill suggestions, ask for inspiration, need help identifying automation opportunities, or request analysis of their chat patterns.
---

# Skill Idea Generator

## Overview

Generate targeted Skill ideas through three complementary approaches: structured discovery interviews, category-based random inspiration, or automated chat history analysis to identify repetitive patterns worth automating.

**Keywords**: skill ideas, automation opportunities, workflow analysis, chat history patterns, inspiration, skill brainstorming, task automation, productivity, skill suggestions

## Core Capabilities

### 1. Random Idea Generation

Serve inspiration-driven Skill ideas when users request random suggestions.

**Trigger phrases:**
- "Give me a random Skill idea"
- "Suggest something fun to build"
- "What's a creative Skill I could make?"
- "Inspire me with a Skill idea"

**Process:**

1. **Determine category preference** (if not specified):
   - Ask: "What type of Skill idea interests you? (fun/productivity/creative/technical/social/experimental)"
   - Or generate ideas across multiple categories

2. **Generate 1-3 ideas** with structure:
   - **Name**: Catchy, descriptive skill name
   - **Purpose**: One-line explanation
   - **Difficulty**: Easy/Medium/Hard
   - **Build time estimate**: Minutes or hours
   - **Use case**: When you'd actually use it
   - **Fun factor** or **Impact score**: High/Medium/Low

3. **Offer follow-up options**:
   - "Want another random idea?"
   - "Want me to flesh out this one with implementation details?"
   - "Want ideas in a different category?"

**Example categories for random generation:**
- **Fun**: Absurd but entertaining (Band Name Generator, Roast Mode, Movie Mashup)
- **Productivity**: Time-savers (Meeting Notes Structurer, Email Thread Summarizer)
- **Creative**: Content tools (Vibe Spec Writer, Moodboard Curator, Story Beat Analyzer)
- **Social**: Communication helpers (Vibe Check Analyzer, Compliment Generator, Text Style Cloner)
- **Technical**: Dev tools (API Documentation Humanizer, Code Review Checklist Generator)
- **Experimental**: Weird combinations (Idea Mutation Machine, Conspiracy Theory Builder)

See `references/random-idea-categories.md` for extensive lists by category.

### 2. Chat History Analysis

Identify Skill opportunities by analyzing conversation patterns using the `conversation_search` and `recent_chats` tools.

**Trigger phrases:**
- "Analyze my chat history for Skill ideas"
- "What repetitive tasks do I do?"
- "Find patterns in my conversations"
- "What should I automate based on my workflow?"

**Process:**

1. **Gather conversation data:**
   ```
   Use recent_chats tool with n=20 multiple times to get broad coverage
   Use conversation_search for specific topics if user mentions areas
   ```

2. **Analyze for patterns:**
   - **Repetitive phrasing**: "format this as...", "make it more...", "apply brand voice"
   - **Consistent formats**: Same output structures requested repeatedly
   - **Domain clustering**: Multiple conversations about same topics
   - **Context re-explanation**: Same background info provided across chats
   - **Manual reformatting**: User frequently tweaks Claude's outputs the same way
   - **Iteration patterns**: Predictable refinement sequences

3. **Categorize opportunities:**
   - **HIGH-PRIORITY**: Frequent (10+ occurrences), clear pattern, high time cost
   - **MODERATE**: Occasional (5-9 occurrences), recognizable pattern
   - **QUICK WINS**: Simple automation, low effort, instant value

4. **Generate structured output:**

```
📊 CHAT HISTORY ANALYSIS
Analyzed: [X conversations over Y timeframe]

🔥 HIGH-PRIORITY OPPORTUNITIES:

1. "[Skill Name]"
   Pattern detected: [description]
   Frequency: [X times in Y chats]
   Time saved: [estimate per use]
   
   Example occurrences:
   - [Chat excerpt showing pattern]
   - [Another example]
   
   Proposed Skill components:
   - SKILL.md: [what it would contain]
   - scripts/: [any automation needed]
   - references/: [reference material]

2. [Next opportunity...]

💡 MODERATE OPPORTUNITIES:
[Similar structure, briefer]

🎯 QUICK WINS:
[Simple one-line suggestions]

---
Would you like me to:
- Build a template for any of these Skills?
- Dig deeper into a specific pattern?
- Analyze a different time period?
```

5. **Proactive suggestion mode** (optional):
   When patterns are detected during normal conversation, offer:
   ```
   💭 Pattern detected: This is the [Xth] time you've asked me to [action].
   
   Would you like me to create a "[Skill Name]" Skill so you can just 
   say "[trigger phrase]" next time?
   
   [Yes, build it] [Not now] [Never suggest this]
   ```

**Analysis strategies:**
- Look for verbs that appear frequently ("format", "summarize", "convert", "analyze")
- Identify output types requested repeatedly (tables, bullet lists, specific structures)
- Notice domain-specific conversations (sales, design, coding, writing)
- Find temporal patterns (weekly reports, daily summaries, regular check-ins)
- Spot tone adjustments ("make it more casual", "add enthusiasm", "formal version")

### 3. Discovery Interview

Guide users through structured discovery when they have a vague automation need.

**Trigger phrases:**
- "Help me figure out what Skill to build"
- "I want to automate something but don't know what"
- "What Skill would help my workflow?"

**Process:**

1. **Start with open questions** (ask 2-3, not all at once):
   - "What task did you do twice this week that felt identical?"
   - "What do you find yourself explaining to me repeatedly?"
   - "What format do you always want output in but have to specify each time?"
   - "What's something you wish Claude just 'remembered' about your preferences?"
   - "What decision do you make frequently that follows a pattern?"

2. **Identify repeatability signals:**
   - Frequency: Daily, weekly, occasionally?
   - Consistency: Same steps every time?
   - Complexity: Simple or multi-step?
   - Context: Requires background knowledge?

3. **Analyze automation potential:**
   - Can this be structured as a repeatable process?
   - What would vary vs. stay constant?
   - What resources would be needed (scripts, references, assets)?

4. **Generate Skill proposal:**
   ```
   Based on your workflow, here's a Skill idea:
   
   🎯 "[Skill Name]"
   
   Purpose: [One-liner]
   Complexity: [Easy/Medium/Hard]
   Build time: [Estimate]
   
   Components needed:
   - SKILL.md: [Core instructions]
   - scripts/: [Any automation scripts]
   - references/: [Background knowledge files]
   - assets/: [Templates or resources]
   
   You'd invoke it with: "[example trigger phrase]"
   
   Want me to build this now or explore other options?
   ```

### 4. Skill Architecture Planning

When user wants to build a specific Skill, help architect it properly.

**Process:**

1. **Clarify the use case:**
   - Get 2-3 concrete examples of how it will be used
   - Understand input types and desired outputs
   - Identify edge cases or variations

2. **Determine resource needs:**
   - **Scripts**: Repetitive code that should be pre-written?
   - **References**: Background knowledge or documentation?
   - **Assets**: Templates, files, or resources to reuse?

3. **Assess complexity level:**
   - **Low freedom** (specific scripts): When operations are fragile or must be exact
   - **Medium freedom** (pseudocode/parameters): When a pattern exists with variation
   - **High freedom** (text instructions): When multiple approaches work

4. **Generate implementation plan:**
   ```
   📋 SKILL ARCHITECTURE: [Name]
   
   Core purpose: [One sentence]
   
   Directory structure:
   skill-name/
   ├── SKILL.md (main instructions)
   ├── scripts/
   │   └── [specific scripts needed]
   ├── references/
   │   └── [documentation files]
   └── assets/
       └── [templates or resources]
   
   SKILL.md sections:
   1. Overview
   2. [Main workflow or capabilities]
   3. [Usage examples]
   
   Key design decisions:
   - [Freedom level and why]
   - [Progressive disclosure strategy]
   - [When to load references vs. keep in SKILL.md]
   
   Ready to build? I can generate the starter files.
   ```

## Anti-Patterns to Avoid

**Don't suggest Skills for:**
- **One-time tasks**: If it won't be used repeatedly, just do it directly
- **Highly variable tasks**: If every instance is completely different, structured automation won't help
- **Better solved by MCP**: Tasks requiring authentication, external services, or real-time data
- **Better solved by external tools**: When a dedicated app already does it perfectly
- **Over-engineering simple preferences**: Just save a prompt template instead

**Red flags in user requests:**
- "I've only done this once but..." → Probably not worth a Skill yet
- "It's different every time..." → May not be automatable
- "I need to connect to [external service]..." → Consider MCP instead
- "Just remember my style..." → This might be better as context/memory

## Usage Examples

**Example 1: Random inspiration**
```
User: "Give me a fun Skill idea"

Claude: 
🎲 Random Skill Idea: "Meeting Vibe Detector"

Analyzes meeting transcripts or calendar invites and predicts the 
"vibe" of the meeting (productive, chaos, rubber-stamp, ideas session, etc.)

Difficulty: Medium
Build time: ~45 minutes
Fun factor: High
Usefulness: Medium

Could include a "survival tips" generator for each vibe type!

Want another idea, or want me to flesh this one out?
```

**Example 2: Chat history analysis**
```
User: "Analyze my chats for automation opportunities"

Claude: [uses recent_chats and conversation_search tools]

📊 I analyzed your last 50 conversations. Here's what I found:

🔥 HIGH-PRIORITY SKILL OPPORTUNITIES:

1. "Product Launch Email Generator"
   You've manually formatted product announcement emails 8 times in 3 weeks
   Pattern: Always include [Feature highlights, Customer benefit, CTA, Timeline]
   Time saved: ~20 min per email
   
2. "Code Review Checklist Creator"  
   You've asked for code review checklists 12 times across different languages
   Pattern: Context-specific items based on language and change type
   Time saved: ~10 min per review

Want me to build templates for either of these?
```

**Example 3: Discovery interview**
```
User: "Help me figure out what Skill to build"

Claude: "Great! Let's find the right automation for you.

What task did you do twice this week that felt identical?"

User: "I keep converting meeting notes into structured action items with owners and deadlines"

Claude: "Perfect! That's very automatable. A few quick questions:

1. Are your meeting notes usually in the same format (transcripts, bullet points, etc.)?
2. Do you always want the same output structure for action items?
3. Is there other info you extract besides actions (decisions, blockers, next meeting)?

This will help me design the right Skill for you."
```

## Progressive Disclosure

This Skill uses minimal context by default. Load references only when:
- User requests ideas in specific categories → Load `references/random-idea-categories.md`
- User wants to learn Skill design patterns → Load `references/skill-design-patterns.md`
- Building vibecoding-specific Skills → Load `references/vibecoding-skills.md`

## When to Use This Skill

**✅ Use this Skill when:**
- User explicitly asks for Skill ideas or inspiration
- User mentions wanting to automate something but seems unsure what
- User asks about workflow optimization or productivity improvements
- User requests analysis of their repetitive tasks
- User mentions building Skills but needs direction

**❌ Don't use this Skill when:**
- User wants to build a specific Skill they already have in mind (use skill-creator instead)
- User is asking about existing Skills or how to use Skills
- User wants to share or discover community Skills
- Request is about MCP servers or external tool integration

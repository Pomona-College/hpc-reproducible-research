# Learner Profiles: Reproducible Research Pipelines

These learner profiles represent the diverse researchers and educators who participate in the Reproducible Research Pipelines workshop. They bring different perspectives on why reproducibility matters and what they hope to gain.

## Profile 1: Dr. Elena Rodriguez, Burned-Out Faculty Reviewer

**Background:** Associate Professor in Environmental Science, 15 years research experience, active as journal reviewer

**Reproducibility Experience:** Hard-earned lessons from failure
- Recently reviewed paper: couldn't reproduce authors' results with provided code
- Spent days contacting authors for missing scripts, data, environment details
- Authors couldn't reproduce their own work from 18 months prior
- Frustrated and skeptical that better approaches are possible
- Under publication pressure herself: worried reproducibility means extra work

**Pain Points:**
- Maintains large pipeline of 12+ projects with collaborators
- Colleagues ask "Can you send me that script from 3 years ago?": often lost or incomprehensible
- Spent weeks debugging paper revisions due to accidentally using wrong dataset version
- Afraid of losing funding if reproducibility standards cost too much time
- Worried students/postdocs won't follow best practices if it slows them down

**What She'll Learn:**
- That reproducibility actually **saves** time long-term
- Practical tools and structures that don't require heroic effort
- How to gradually implement in existing projects
- That documentation is easier when done incrementally
- How to convince collaborators it's worth the effort

**Key Insight She Needs:**
"Every hour spent making a pipeline reproducible is an hour you save next year when you need to rerun it, modify it, or defend your results."

**Example Scenario:**
```
Before: Spent 2 weeks tracking down why revision results changed
After: With reproducible setup, identified the data version difference in 2 hours
```

---

## Profile 2: Dr. James Powell, Detail-Oriented Postdoc

**Background:** Postdoctoral researcher in Biostatistics, preparing first-author manuscript

**Reproducibility Experience:** Wants to do it right
- Meticulous researcher, but no formal training in reproducible practices
- Manuscript under review: editor requires code availability
- Has analysis scripts but organized messily
- Wants to set gold standard for publication but unsure best practices
- Worried about releasing code with errors or ambiguities

**Pain Points:**
- Scripts reference hardcoded paths that won't work on others' computers
- Downloaded datasets aren't version-controlled
- Made small tweaks during analysis: can't remember original vs. final parameters
- Collaborators on paper can't understand script structure
- Editor wants supplementary materials but isn't sure what to include
- Fear that making code public might expose mistakes

**What He'll Learn:**
- How to organize code for publication (real supplementary materials)
- Version control (git) for tracking analysis decisions
- Environment specification (conda, containers) for reproducibility
- Clear documentation practices without over-engineering
- How to handle data availability properly
- That imperfect but transparent code is better than hidden perfect code

**Key Insight He Needs:**
"Publishing your actual code, with all its quirks, builds trust with readers more than hiding implementation details."

**Example Scenario:**
```
Before: 47 scripts in ~/analysis, unclear which produce published results
After: Organized repo with clear pipeline, README, and data versioning
Result: Paper published with reproducible supplement cited 15 times for methodology
```

---

## Profile 3: Maria Gonzalez, Undergrad Learning Early

**Background:** Junior Computer Science major, part-time research assistant in lab

**Reproducibility Experience:** First exposure to real research
- Smart coder from CS coursework but new to scientific computing
- Excited to help with research but unsure of standards
- Tends to write quick code, want to improve practices
- Advisors haven't emphasized reproducibility formally
- Sees opportunity to learn right way before bad habits form

**Pain Points:**
- Codes solution quickly without documenting it
- Lab PI can't understand her script months later when needed
- Doesn't know what "version control" actually means practically
- Uses filenames like `analysis_final_v2_REAL_FINAL.py`
- Nervous about being judged if code isn't perfect
- Doesn't know industry standard practices

**What She'll Learn:**
- Practical git workflow (why it matters, not just how)
- Clean code practices adopted by real scientists
- Documentation that helps others (including future self)
- Testing and verification approaches
- How to organize projects professionally
- That reproducibility is an expected norm, not optional extra

**Key Insight She Needs:**
"Learning reproducibility practices now will make you more valuable as a researcher and engineer than most students at your level."

**Example Scenario:**
```
Before: Had to rewrite analysis code because original was incomprehensible
After: With clean code and documentation, modified existing script in 30 minutes
Bonus: Lab PI now trusts her with more complex work
```

---

## Profile 4: Dr. Kwame Osei, Under-Resourced Graduate Student

**Background:** Fifth-year PhD student in Machine Learning, limited computing resources

**Reproducibility Experience:** Wants reproducibility but constrained by circumstances
- Working on thesis with finite computational budget
- Runs experiments on shared Sagehen HPC cluster (limited GPU hours)
- Multiple experiments running in parallel with different hyperparameters
- Collaborating with another student who writes code differently
- Needs to submit code for conference reviews but rushed

**Pain Points:**
- Can't re-run all experiments due to GPU hour limits
- Made tweaks to improve performance: documented informally in email
- Different students use different conda environments (library version conflicts)
- Reviewer feedback months later hard to implement (forgot setup)
- Conference submission deadline in 2 weeks
- Worried reproducing code will take computational resources he can't spare

**What He'll Learn:**
- Minimal reproducibility practices that fit resource constraints
- How to document what matters most (hyperparameters, seed, environment)
- Separating reproducible parts from computationally expensive parts
- Using supplementary materials strategically (what to include, what's optional)
- Making reproducible without re-running full pipelines
- That conference reviewers appreciate reproducibility docs even if re-computation isn't feasible

**Key Insight He Needs:**
"You can be reproducible with limited resources; focus on what matters: parameters, code logic, and results claiming."

**Example Scenario:**
```
Before: Reviewer asked to verify results: couldn't, lost experiment logs
After: Document hyperparameters + seed + model architecture: reviewer replicates in 1 hour
Result: Acceptance because reproducibility evidence was clear
```

---

## Profile 5: Dr. Aisha Williams, Advisor Promoting Culture Change

**Background:** Lab PI with 8 years experience, training next generation of researchers

**Reproducibility Experience:** Advocating for change across a lab
- Personally frustrated by irreproducible research in field
- Wants lab to be known for reproducible science
- Responsible for training 3 grad students + 4 undergrads
- Varies in technical skills and background
- Budget constraints on computing and services
- Needs practical strategies that work for diverse team

**Pain Points:**
- Different students learn different practices before joining lab
- Enforcing standards takes time she doesn't have
- Worried losing students to labs with fewer requirements
- Unsure what "reproducible" actually means in her field
- Needs to convince students it's worth the effort
- University systems don't support reproducible practices well

**What She'll Learn:**
- How to create lab culture around reproducibility
- Reproducible practices scalable across team skill levels
- Tools that integrate with existing workflows (not new burden)
- How to gradually implement without disrupting current work
- Making the business case to students and funding agencies
- How to review reproducibility without being bottleneck

**Key Insight She Needs:**
"Lab reproducibility standards aren't extra work; they're the default way of working that saves time for everyone."

**Example Scenario:**
```
Before: New postdoc spends 2 weeks understanding previous grad student's pipeline
After: With lab repo + documentation standards: 2 days to understand and modify
Multiplied by 6 people rotating through lab yearly: Saves months of person-hours
Result: Lab publishes more, trains better, researchers more productive
```

---

## Profile 6: Dr. Tomás García, Computational Expert Who Doesn't Know It Yet

**Background:** Senior researcher, primarily experimental background, now learning computational methods

**Reproducibility Experience:** Recently transitioned to computation-heavy work
- Expert experimentalist but new to computational reproducibility
- Relied on collaborators for analysis code
- Now leading computational experiments for first time
- Worried about credibility in new domain
- Sees reproducibility as experimental scientists see lab notebooks (essential)
- Strong domain knowledge but learning computing culture

**Pain Points:**
- Not trained in software practices (git, containers, etc.)
- Afraid of looking incompetent asking about basic tools
- Huge learning curve (she'll catch up quickly though)
- Wants to set reproducibility standard but unsure how in computing context
- Collaborators using different tools/languages than her training
- Needs to understand enough to lead research team

**What She'll Learn:**
- That reproducibility practices align with scientific method rigor she knows
- Git is like experiment log + version control combined
- Containers/environments are like "recording exact lab setup"
- Documentation in computation = lab notebook + methods section
- That she can learn these tools as well as others
- Leadership in reproducibility doesn't mean technical expertise in all tools

**Key Insight She Needs:**
"Computational reproducibility is exactly like experimental rigor: documenting setup, methods, and results. You already know how to do this in the lab."

**Example Scenario:**
```
Before: Computational collaborator's analysis opaque, hard to build on
After: With reproducible practices, results are credible and extensible
Result: Other labs cite her work and ask to collaborate on similar analyses
```

---

## Teaching Strategy by Profile

### For Dr. Rodriguez (Faculty Reviewer):
- Lead with time-saving benefits
- Show real examples of past problems reproducibility would solve
- Discuss return on investment in long projects
- Emphasize that her frustration with irreproducible work is why she's teaching
- Give her quick wins (one small project to make reproducible)

### For Dr. Powell (Postdoc Publishing):
- Focus on publication-ready practices
- Provide publication supplement checklist
- Discuss data availability strategies
- Show how to handle proprietary/sensitive data responsibly
- Frame as "polishing for publication" not "extra work"

### For Maria (Undergrad):
- Emphasize that these are industry standards she's learning early
- Show how this makes her more employable post-graduation
- Build on her programming knowledge from CS classes
- Make it aspirational: "This is what great researchers do"
- Less focus on theory, more on practical workflow

### For Dr. Osei (Constrained Graduate Student):
- Address resource limits directly
- Show minimal-viable reproducibility for his constraints
- Separate "nicely reproducible" from "just barely shareable"
- Discuss smart documentation (what matters most)
- Provide templates for quick setup
- Reassure: reproducibility doesn't mean re-running everything

### For Dr. Williams (Lab PI):
- Frame as lab culture and onboarding
- Provide templates and checklists for team use
- Discuss how to review for reproducibility
- Show how to gradually adopt without disruption
- Emphasize mentoring benefits: students learn professional practices
- Help her make the argument to her team

### For Dr. García (Transitioning Expert):
- Validate her experimental rigor and apply same thinking to computation
- Use analogies to experimental laboratory practices
- Emphasize that she's applying existing expertise to new domain
- Don't assume she knows command line; provide gentle intro
- Build her confidence: she's more qualified than she thinks

---

## Workshop Impact by Profile

| Profile | Core Motivation | Primary Takeaway | Expected Change |
|---------|-----------------|------------------|-----------------|
| Dr. Rodriguez | Reviewer frustration | Saves future time & sanity | Implements in next project |
| Dr. Powell | Publication standards | Publication-ready practices | Publishes reproducible supplement |
| Maria | Learning right way | Professional practices as norm | Carries throughout career |
| Dr. Osei | Constrained resources | Minimal reproducibility | Reviewer confidence |
| Dr. Williams | Lab culture | Sustainable team practices | Lab reproducibility standard |
| Dr. García | Domain transition | Computational practices parallel lab rigor | Research credibility |

---

## Cross-Cutting Themes

All six profiles share underlying concerns that the workshop addresses:

### 1. **Reproducibility is Possible Within Real Constraints**
- Not just for well-funded labs
- Not just for computer scientists
- Not requiring abandoning current work
- Fits different timelines and resource levels

### 2. **Reproducibility Saves Time (Eventually)**
- Short-term cost (learning, setup)
- Long-term benefit (months/years saved)
- Multiplies across team members
- Compounds with project duration

### 3. **Reproducibility is About Communication**
- With reviewers and editors
- With collaborators
- With future self
- With scientific community

### 4. **Credibility and Trust**
- Transparent methods build trust
- Code sharing demonstrates rigor
- Reproducible results convince skeptics
- Documentation reduces suspicion of errors

---

## Facilitating Peer Learning

A strength of this workshop is the diversity of experiences. Encourage:

- **Dr. Rodriguez ↔ Dr. Powell:** Former asks how to evangelize to collaborators; Powell explains publication angle
- **Maria ↔ Dr. Williams:** Undergrad learns expectations from PI; PI gains perspective on entry-level understanding
- **Dr. Osei ↔ Dr. García:** Resource-constrained researcher helps transitioning expert; García mentors on field-specific practices
- **All:** Share field-specific reproducibility examples and war stories

These cross-profile conversations often yield the deepest learning and most practical adaptations.

---

## Inclusivity Notes

This diverse group ensures the workshop remains:
- **Relevant** to career stages from student to senior faculty
- **Practical** for field-specific and resource-specific constraints
- **Motivating** through shared pain points and genuine examples
- **Supportive** (no one is an outsider; everyone is learning)
- **Encouraging** (if this person can do it, I can too)

Teaching reproducible research is teaching good science. This workshop serves everyone who wants to do better science, regardless of their starting point.

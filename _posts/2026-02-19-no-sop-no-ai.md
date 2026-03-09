---
title: "No SOP, No AI Use Case"
date: 2026-03-09 09:00:00 +0000
categories:
  - AI-implementation
tags:
  - evals
  - implementation
  - enterprise
  - leadership
  - SOP # NEW SUGGESTION
  - workflow
---

It's simple: no documented workflow, no AI use case.

{% comment %}
![Checklist showing SOP, data readiness, and golden examples as prerequisites before AI development begins]({% link "relative path" %}){: style="border: 1px solid #2E8B57;"}

<p style="text-align: center; font-size: 0.9em; color: #555;">
  Source: Source
</p>
{% endcomment %}

A pattern I keep encountering in stalled AI projects: the business reports "the AI is outputting wrong results." Upon closer inspection, the AI is doing exactly what it was asked to do — it was just asked incorrectly. The sequence of steps is wrong, the calculations are fuzzy, the data source is off. The root cause isn't the model. It's that nobody documented what "right" looks like before development started.

Teams routinely assume the model will figure out what to do without the hard work of defining success. It won't. The project pauses, developers sit idle, and the business scrambles to reverse-engineer the very workflow it should have documented upfront. Without a clear process, AI simply scales and automates existing confusion.

Three prerequisites before greenlighting any use case:

1. **Documented workflow with clear improvement targets.** Map the current process end to end: steps, owners, decision points. Identify specifically where AI adds value. If you can't write the SOP, the AI can't execute it.
2. **Confirmed and accessible data.** Identify the exact systems, fields, and formats required. Verify access and accuracy before engineering begins. Assumed data availability is the fastest path to a stalled project.
3. **Golden examples for the engineering team.** Provide real, validated outputs that represent "good." Development compresses dramatically when engineers build against concrete examples rather than iterating through ambiguous feedback loops.

These three items require real investment from the business, not just the technical team. Process definition, data validation, and example curation are business responsibilities.

AI rewards preparation. It punishes ambiguity.

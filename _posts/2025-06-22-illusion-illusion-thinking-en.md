---
layout: post
title: "The Illusion of the Illusion of Thinking: When AI Evaluation Methods Become Traps for Capability Assessment"
date: 2025-06-22 01:00:00
description: "This commentary paper reveals a shocking truth: we often mistake the limitations of AI evaluation methods for the limitations of AI systems themselves! Research shows that many cases considered AI reasoning failures are actually misjudgments caused by poorly designed evaluation frameworks."
tags: ai-evaluation reasoning model-limitations methodology
categories: paper english
thumbnail: assets/img/paper/The_Illusion_of_Thinking/cover.png
related_posts: true
lang: en
lang_ref: illusion-illusion-thinking-tutorial
---

<div class="language-switcher" style="text-align: right; margin-bottom: 20px; padding: 8px 0; border-bottom: 1px solid #333;">
  <span style="font-size: 14px; color: #888; margin-right: 8px;">🌐 Language:</span>
  <a href="/blog/2025/illusion-illusion-thinking-chs/" style="color: #007bff; text-decoration: none; font-weight: 500; transition: opacity 0.2s;" onmouseover="this.style.opacity='0.7'" onmouseout="this.style.opacity='1'">中文</a>
  <span style="color: #666; margin: 0 6px;">|</span>
  <strong style="color: #007bff; font-weight: 600;">English</strong>
</div>

> **Source:** [Paper](https://arxiv.org/pdf/2506.09250)

This is an important commentary paper by A. Lawson, officially titled **[The Illusion of the Illusion of Thinking: A Comment on Shojaee et al. (2025)](https://arxiv.org/pdf/2506.09250)**. This paper responds to Shojaee et al. (2025)'s research on Large Reasoning Model limitations, which builds upon the Apple paper we previously analyzed. If you're interested in this background, you can refer to our previous notes: [The Illusion of Thinking: Apple Paper Analysis](/blog/2025/illusion-thinking-en/).

This commentary presents a profound perspective: **we often mistake the limitations of evaluation methods for the limitations of AI systems themselves**.

## Core Problem: The Trap of Evaluation Methods

Shojaee et al. (2025) claimed to have discovered fundamental limitations of Large Reasoning Models through systematic evaluation of planning puzzles, finding that model accuracy "collapses" to zero beyond certain complexity thresholds.

> _"Shojaee et al. (2025) claim to have identified fundamental limitations in Large Reasoning Models through systematic evaluation on planning puzzles. Their central finding—that model accuracy 'collapses' to zero beyond certain complexity thresholds—has significant implications for AI reasoning research."_

However, this commentary paper points out that these apparent failures actually stem from experimental design limitations rather than fundamental reasoning capability defects of the models themselves.

> _"However, our analysis reveals that these apparent failures stem from experimental design limitations rather than fundamental reasoning failures."_

## Three Key Problems

### Problem One: Models Actively Recognize Output Constraints

**Most Important Finding:** Models can actually actively recognize when they approach output limits and make reasonable stopping decisions.

The paper cites a replication experiment by @scaling01 on Twitter, which captured model outputs explicitly stating:

> _"The pattern continues, but to avoid making this too long, I'll stop here"_

This proves that models understand the solution pattern but choose to truncate output due to practical constraints.

> _"This demonstrates that models understand the solution pattern but choose to truncate output due to practical constraints."_

**Key Insight:** Misjudging this model behavior as "reasoning collapse" reflects a fundamental problem with automated evaluation systems that fail to account for model awareness and decision-making capabilities.

### Problem Two: Mathematical Analysis of Token Limitations

The paper provides detailed mathematical analysis explaining why Tower of Hanoi problems encounter token limitations:

**Linear Growth Pattern (outputting only final sequence):**

```
T_final(N) ≈ 10(2^N - 1) + C
```

**Maximum Solvable Scale:**

```
N_max ≈ log₂(L_max/10)
```

For different token limits:

- L_max = 64,000: N_max ≈ 12-13
- L_max = 100,000: N_max ≈ 13

**Key Finding:** The reported "collapse" occurs before N = 9, well before these theoretical limits.

> _"Interestingly, the reported 'collapse' before N = 9 for most models occurs well before these theoretical limits. This suggests that models are making a decision to terminate output before actually reaching their context window limits."_

### Problem Three: The Trap of Impossible Puzzles

In River Crossing experiments, evaluation issues are dramatically compounded:

> _"The evaluation issues are compounded dramatically in the River Crossing experiments. Shojaee et al. test instances with N ≥ 6 actors/agents using boat capacity b = 3. However, it is a well-established result that the Missionaries-Cannibals puzzle (and its variants) has no solution for N > 5 when b = 3."_

**Problem Essence:** The evaluation framework includes mathematically impossible problems, then attributes models' inability to solve these problems to insufficient reasoning capabilities.

## Experimental Validation: Alternative Representations Restore Performance

To test whether failures reflect reasoning limitations or format constraints, the author conducted preliminary testing:

**Test Prompt:**

```
"Solve Tower of Hanoi with 15 disks. Output a Lua function that prints the solution when called."
```

**Surprising Results:**

- Multiple advanced models (Claude-3.7-Sonnet, Claude Opus 4, OpenAI o3, Google Gemini 2.5) achieved very high accuracy
- Completed within 5,000 tokens
- Generated solutions correctly implemented recursive algorithms

> _"The generated solutions correctly implement the recursive algorithm, demonstrating intact reasoning capabilities when freed from exhaustive enumeration requirements."_

## Reevaluating Complexity Claims

The paper points out problems with the original research using "compositional depth" (minimum moves) as a complexity metric:

| Puzzle         | Solution Length | Branching Factor | Computational Complexity                  |
| -------------- | --------------- | ---------------- | ----------------------------------------- |
| Tower of Hanoi | 2^N - 1         | 1                | O(1) per move                             |
| Blocks World   | O(N)            | O(N²)            | Linear (near-optimal) / NP-hard (optimal) |

**Key Insight:** Tower of Hanoi, despite requiring exponentially many moves, has a trivial O(1) decision process per move. Blocks World is much harder.

### The Optimality Question

Blocks World prompts explicitly require optimization:

> _"Find the minimum sequence of moves to transform the initial state into the goal state."_

While the solution checker only verifies correctness rather than optimality, models attempt to follow instructions to find optimal solutions, increasing computational burden.

## Deep Thinking: The Importance of Evaluation Design

### Engineering Decisions vs Reasoning Failures

The paper's most innovative insight is distinguishing between "engineering decisions" and "reasoning failures":

> _"This distinction further emphasizes the importance of evaluation design. Scoring models as 'failures' for making reasonable engineering decisions about output length mischaracterizes their actual capabilities."_

**Core Viewpoint:** When models choose not to output lengthy intermediate steps, this may be reasonable efficiency consideration, not capability defect.

### Models' Self-Awareness Capabilities

Research found models possess some form of self-calibration ability:

> _"This behavior indicates that models may be poorly calibrated about their own context length capabilities, choosing to stop prematurely."_

**Important Finding:** Models demonstrate some form of self-awareness about their capabilities, making stopping decisions based on circumstances.

## Profound Impact on AI Evaluation

This research reminds us to rethink AI evaluation methodology:

**Future work should:**

1. **Design evaluations that distinguish between reasoning capability and output constraints**
2. **Verify puzzle solvability before evaluating model performance**
3. **Use complexity metrics that reflect computational difficulty, not just solution length**
4. **Consider multiple solution representations to separate algorithmic understanding from execution**

### Philosophy of Measurement Problems

This reminds me of the philosophy of measurement problems: our measurement tools and methods themselves influence our understanding of the measured objects.

**Core Insight:** In AI evaluation, we need to more carefully design evaluation frameworks to avoid mistaking methodological limitations for system capability boundaries.

## Analogy with Human Cognition

### Efficiency-Priority Intelligent Behavior

This research reminds me of human cognitive patterns when facing complex problems:

- **Pragmatism** - Humans also use heuristic methods rather than brute force enumeration
- **Cognitive Load Management** - Humans adjust thinking strategies based on problem complexity
- **Self-Monitoring** - Humans are aware of their cognitive limitations and adjust accordingly

**Deep Thinking:** Does this behavior of AI models reflect some characteristic of intelligence?

## Critical Thinking

### The Importance of Balance

While this commentary paper presents valuable viewpoints, we also need to note:

**Avoid Extremism:** Don't completely ignore real limitations of AI systems because of criticizing original research

**Maintain Objectivity:** Need to establish more balanced and nuanced evaluation frameworks that can identify true capability boundaries without misjudging due to methodological issues

## Conclusion

This paper ultimately reminds us of an important truth:

> _"The question isn't whether LRMs can reason, but whether our evaluations can distinguish reasoning from typing."_

**Core Insights:**

- Evaluation method design directly affects our judgment of AI capabilities
- Need to distinguish between true capability limitations and evaluation framework limitations
- AI systems' "failures" may actually be reasonable engineering decisions
- Models' demonstrated self-awareness and adaptive capabilities deserve attention

**In AI capability evaluation, "how to ask questions" is often as important as "what questions to ask."**

This research not only provides important reflection for current AI evaluation but also points the way for building more reliable AI capability evaluation systems in the future. We need to see through the "illusion of evaluation" to understand the true capabilities of AI systems.

---

## References

- [The Illusion of the Illusion of Thinking: A Comment on Shojaee et al. (2025)](https://arxiv.org/pdf/2506.09250)
- [The Illusion of Thinking: Understanding the Strengths and Limitations of Reasoning Models via the Lens of Problem Complexity](https://arxiv.org/pdf/2506.06941)

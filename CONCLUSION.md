# Lab 05 — Conclusion

<!--
Read this before you write.

This file is graded by your instructor, by hand, and it is worth 6 of the 15 points.

- Type it yourself. Do not use a chatbot, an AI assistant, an AI autocomplete or a "rewrite my
  text" tool for any part of it, not even for grammar. English that is plain and a little
  clumsy but yours is better than polished text that is not.
- Use your own numbers. Every number you quote must be one your own notebook printed. Copy
  the number exactly as it appeared. A number your notebook did not print is marked as wrong.
- Answer in your own words, about 80-200 words per question. A short answer that uses your
  numbers beats a long answer that could have been written by anyone.
- Delete these comment lines if you like. Keep the three headings and the last line.
-->

## 1. Which layer changed your results the most?

Give the numbers before -> after for that layer (correct / wrong / no answer, out of 20) and say
whether it changed what the agent *can do* or only what it *admits*. If no layer changed the
number of correct answers, say so and explain what the layers did change instead.

Layer 1 changed the numbers the most. Before it (bare), the score was 16 correct, 4 wrong, and 4 no answer. After Layer 1, it became 18 correct, 6 wrong, and 0 no answer. Layer 1 helped the agent get a little smarter because instead of crashing on alias questions, it fixed two region names (q23 and q24). At the same time, it picked the wrong regions on q21 and q22, so wrong answers also went up. Layer 2 did not change anything at all, and Layer 3 just made the model more honest by turning 2 wrong answers into no answer.

## 2. One failure the full harness did not fix

Pick one question that the full harness (layer 3) still gets wrong or refuses. Give its id
(q01 ... q20). Open its trace in the notebook: at which step did it go wrong, and what did
the model do instead? Name one thing you would add to the harness, and what it would cost
(extra model calls, tokens, time, or a new way to fail).

Question q21 (unemployment rate in Nur-Sultan in 2022) is still wrong even with Layer 3. In the trace, the model first called get_series with Nur-Sultan and got an unknown region error. Then it called list_regions, saw the valid names, but did not realize that Nur-Sultan is now Astana, so it picked a different region and returned its number. Layer 3 allowed this because the number came from a tool, even though it was for the wrong city. To fix this, we can add a simple alias map in the harness to replace old names (like Nur-Sultan to Astana) before calling the tool. The cost of this fix is a new way to fail, because if the map guesses wrong on similar names (like Almaty City and Almaty Region), it will quietly return data for the wrong place.

## 3. Workflow or agent for the monthly ranking?

Dala Insight wants to publish, every month, "which region is highest and which is lowest on one
indicator in one year". Would you ship the fixed workflow or the agent? Use the accuracy, the
number of model calls, the tokens and the time you measured in Part 1. Then say in one sentence
what kind of question would make you choose the other one.

I would pick a fixed workflow because the question format is always the same. Looking at the 15 fixed questions in Part 1, the workflow is much better: it got 13/15 correct (compared to 9/15 for the bare agent), used only 15 model calls instead of 26, spent 3,852 tokens instead of 19,835 (about 5 times cheaper), and finished in 15.8 seconds instead of 50.2 seconds. I would only choose an agent if the firm needed to answer open multi-step questions, such as asking how many percent the average wage grew from 2020 to 2025 in the region that had the highest unemployment rate in 2025

---

I wrote this file myself, without any AI tool.

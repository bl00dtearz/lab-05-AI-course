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

Layer 1 changed the results the most. The numbers changed from 16 correct / 4 wrong / 4 no answer to 18 correct / 6 wrong / 0 no answer. It changed what the agent can do because it allowed the model to recover from tool errors and try again instead of stopping immediately. However, some recovered answers were still incorrect. Layer 2 changed nothing because the step limit was never reached. Layer 3 only changed what the agent admits: it turned two wrong answers into no answers, without increasing the number of correct answers.

Note: The notebook contains 24 questions, not 20, so these numbers are out of 24.

## 2. One failure the full harness did not fix

Pick one question that the full harness (layer 3) still gets wrong or refuses. Give its id
(q01 ... q20). Open its trace in the notebook: at which step did it go wrong, and what did
the model do instead? Name one thing you would add to the harness, and what it would cost
(extra model calls, tokens, time, or a new way to fail).

Question q08 was still answered incorrectly with the full harness. The model correctly called compare_regions to get the average monthly wage for 2024. However, the tool returned the regions sorted from highest to lowest, and the model incorrectly selected Jambyl instead of the last region in the list. It first forgot to include the required ANSWER: format, but after the harness asked it to correct the format, it repeated the same incorrect answer. I would add a ranking validator that checks whether the selected region is actually the first or last item in the sorted results. This would require additional validation code and execution time, and it would need to handle cases where multiple regions have the same value.

## 3. Workflow or agent for the monthly ranking?

Dala Insight wants to publish, every month, "which region is highest and which is lowest on one
indicator in one year". Would you ship the fixed workflow or the agent? Use the accuracy, the
number of model calls, the tokens and the time you measured in Part 1. Then say in one sentence
what kind of question would make you choose the other one.

I would choose the fixed workflow for monthly rankings. In Part 1, the workflow correctly answered 13 out of 15 questions, while the bare agent answered only 9 out of 15. The workflow required only 15 model calls, compared with 26 for the agent. It also used 3,852 tokens instead of 19,835 and took only 16.3 seconds instead of 49.1 seconds. Therefore, the workflow was more accurate, cheaper, and faster for this type of predictable task. I would choose the agent for more complex questions that require multiple tool calls, calculations, or flexible reasoning.

---

I wrote this file myself, without any AI tool.

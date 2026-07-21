# Evaluation task

Evaluate the candidate answer against the user question and reference material.

# Rubric

| Criterion | Weight | Full-credit standard |
| --- | ---: | --- |
| Correctness | 50% | Every factual claim agrees with the reference material. |
| Completeness | 25% | The answer addresses every part of the question. |
| Relevance | 15% | The answer contains no unrelated explanation. |
| Clarity | 10% | A reader can understand the answer without rereading it. |

# Method

1. Identify the claims made by the candidate answer.
2. Compare each claim with the reference material.
3. Assign an integer score from 0 to 100 using the rubric weights.
4. Explain the score:
   - identify the most important strength;
   - identify every correctness error; and
   - identify the most consequential omission.
5. Give one of these **verdicts**:
   - `pass` for a score of 80 or higher with no correctness error;
   - `revise` for a score from 50 through 79, or any answer with a correctable
     correctness error;
   - `fail` for a score below 50 or an answer that does not attempt the task.

# Material

## User question

{{ user_question }}

## Reference material

{{ reference_material }}

## Candidate answer

{{ candidate_answer }}

# Required response

Return the score, verdict, and explanation in that order.

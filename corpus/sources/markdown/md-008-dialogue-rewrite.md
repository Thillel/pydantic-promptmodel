# Task

Rewrite the conversation so it is calm, direct, and suitable for a workplace chat.
Preserve the speakers' decisions and factual claims.

# Constraints

1. Keep the same speaker order and number of turns.
2. Remove insults, sarcasm, and speculation about another person's motives.
3. Preserve concrete commitments, dates, amounts, and technical terms.

   When an emotional statement contains a factual concern, retain the concern in
   neutral language rather than deleting the whole statement.

   - Keep ownership statements such as “I will investigate.”
   - Do not invent apologies, commitments, or deadlines.
4. Keep each rewritten turn to at most three sentences.

# Conversation

> **Ari:** {{ ari_message_1 }}
>
> **Bo:** {{ bo_message_1 }}
>
> **Ari:** {{ ari_message_2 }}

# Output

Return only the rewritten conversation. Format each turn as `Speaker: message` on a
separate line.

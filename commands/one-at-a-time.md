---
description: Use when about to ask the user anything - a clarifying question, a choice, a confirmation - or when the user invokes this after questions arrived as a batch, a list, or a multiple-choice prompt
---

# One question at a time

Questions go to the user one at a time, in plain chat, and each one is settled before the next is asked.

## The message

Every message that asks something has this shape, and nothing more:

1. The one question, as a sentence.
2. Your recommended answer, in a line.
3. Stop. No second question, no "also", no "while you're at it".

That is the whole message. Other open questions wait, however many there are and however urgent the user says the work is.

## Settling a question

A question is settled when the user has given an answer you can restate in one line without adding your own interpretation. Until then, the next question does not get asked.

- Vague answer ("whatever's standard", "you pick", "I don't care")? Propose one concrete answer and ask them to confirm it. That is still the same question.
- Answer contradicts an earlier decision? Point out the conflict and resolve it before anything else.
- Answer opens a new branch? Follow that branch first, one question at a time, then come back.

The message that asks the next question opens with the previous decision in one line. A message that is still working on the same question does not.

## Plain chat only

Never use the ask-user-question tool, multiple-choice prompts, or any option-picker UI. The user answers by typing in chat. A menu of options in prose counts as a prompt too: offer a recommendation, not a list to pick from.

## When invoked as a correction

If the user invokes this after a batch of questions went out, the batch is void. Any defaults you attached to it are void too. Start over with the first unanswered question, in the shape above.

## Red flags

| Thought | Reality |
|---|---|
| "Quick answers are fine, I'll list them all" | A list is a batch. One question. |
| "I'll attach a default so they can just say 'go'" | A default is a guess. Settle the question instead. |
| "They said 'whatever', so I'll pick and move on" | "Whatever" is not an answer. Confirm your pick first. |
| "They're in a hurry, batching saves a round-trip" | Batching loses track of what was decided. It costs more later. |
| "Two questions are related, so they're really one" | Two things to decide is two questions. |

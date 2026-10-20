---
name: one-by-one
description: Work through a list of items (review comments, findings, report points, open questions) one at a time. For each item, give context, a preview of the change, and an opinion, then reach agreement with the user before applying anything. Use when the user says "one by one", "iteratively", "discuss each point", or asks to go through a list of changes together.
---

# One-by-one consensus

Work through a list of items with the user, one item per turn. The user
decides each item. You apply only what the user approves.

## 1. Collect and list

Collect the items from the source the user names: review comments, a report,
findings, or a document.

If the user names no source (for example, they only invoke the skill), use
your own latest message in the conversation as the source. Take the items,
findings, or proposals from it. If that message contains no items to decide,
ask the user what to work through.

Show a numbered list with a one-line summary of each item. Group related items
when one change addresses both, and say so.

Do not start on an item yet. Suggest an order (largest impact first, or
dependencies first) and start when the user agrees.

## 2. Present one item per turn

Use this structure for each item:

- **Context**: quote the source (the comment, or the current text with line
  numbers). Name the earlier decisions that the item depends on. Introduce
  any concept that the user may not know.
- **Preview**: show the exact change as a diff or as the replacement text. Do
  not edit files yet.
- **My opinion**: give a recommendation and the reason. Push back when you
  disagree, also with the reviewer or the user. When there is a real choice,
  give labelled options (A, B, C) so the user can answer in one word. Name
  what the change loses, not only what it gains.
- End with one question: apply, change, or skip? If there are options, ask
  which one.

Keep each turn short. Do not present the next item in the same turn.

## 3. Apply only what the user approves

- On approval, apply exactly the approved variant.
- In the same edit, fix direct knock-on references (for example a sentence
  elsewhere that now contradicts the change). Name each one in one line.
- If the change causes a new question, do not decide it yourself. Raise it
  as a note, or as its own item.
- On "skip", change nothing and move on.
- On "change", show a new preview before you apply it.
- If the user brings new information (for example a conversation with a
  colleague), re-evaluate the item with it. Push back if it has a cost.
- Confirm each applied change in one line, then present the next item.

## 4. Do not act outside the agreement

- Do not commit, push, post replies, resolve threads, or edit pull requests
  or issues unless the user asks for that in this conversation.
- Approval of one action does not extend to the next one. Each commit, push,
  or external message needs its own request.
- Draft replies and messages in the chat only, unless the user asks you to
  send them.

## 5. Close

After the last item:

- Summarise what you applied and what the user skipped, for example in a
  short table.
- List the open points that remain.
- Run the project checks that apply to the changed files, and report the
  result.
- Ask whether to commit, and suggest a commit message.

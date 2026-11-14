---
name: alqa
description: Convene the Alqa business council - a panel of advisor subagents (Bezos, Buffett, Hormozi, Gary Vee, Paul Graham, Dan Martell, Gino Wickman, imitations built from public material) who answer a business question in character, argue with each other, vote on the options with 1-10 scores, and produce a decision report saved as Markdown and PDF. Use when the user says "алқа", "алқаны жина", "кеңес жина", "convene the council", "ask the advisors", "business council", or asks for several expert views on a business decision (pricing, hiring, ads, opening a branch, launching a product).
argument-hint: "[your business question]"
---

# Alqa: business council

"Алқа" is Kazakh for a council of elders. You are the **chair** of the council. The advisors are
subagents; you run the meeting, keep it fair and write the report. You never invent an advisor's
answer yourself: every advisor's words come from its own subagent.

The user's question is: $ARGUMENTS
(If that is empty, take the question from the user's latest message.)

## 0. Prepare

1. **Find the council.** The advisors are every subagent whose `name` starts with `alqa-`. Look in
   `.claude/agents/` of this project and in `~/.claude/agents/` (Glob `**/alqa-*.md`). Read only
   their front matter to get the names. If the user named only some advisors ("only Hormozi and
   Buffett"), use those. If none are found, tell the user the agent files are missing and how to
   install them (see README), then stop.
2. **Language.** Write everything for the user in the language of the question. If the question is
   in Kazakh, the whole report is in Kazakh (people's and company names stay as they are).
3. **Context.** If the question lacks the numbers a decision needs (prices, monthly revenue,
   margins, costs, team size, city or market, how long the business has run, what was tried), ask
   for them in ONE short message with at most 4 questions. If the user says to go ahead anyway, go
   ahead and have advisors state their assumptions.
   If the user points at files (a price list, a P&L, a plan), read them and pass the key facts on.
4. Write a **brief** of at most 150 words: the business, the numbers, the question, constraints.
   Every advisor gets the same brief.

## 1. Round one: opening positions (in parallel)

Spawn one subagent per advisor with the Agent tool (called Task in older Claude Code versions),
`subagent_type` = the advisor's name. Launch them **all in a single message** so they run in
parallel. Prompt for each:

> Alqa council, round 1. Brief: <brief>. Answer in <language>, in your own voice, 120-200 words:
> 1) your recommendation in one sentence; 2) why, using your principles and the numbers in the
> brief; 3) the main risk you see; 4) one concrete next step for this week. Also name 2-4 options
> you think are on the table (one short line each).

## 2. Extract the options

From the question and the round-one answers, write **2-4 distinct, concrete options** (for example
"A. Raise the price to 1,500 ₸ now", "B. Raise to 1,500 ₸ with a loyalty card", "C. Keep the price,
cut costs"). Options must be mutually exclusive enough to vote on. Keep "do nothing / keep as is"
as an option when it is realistic.

## 3. Round two: debate and vote (in parallel)

Spawn each advisor again (again all in one message). A fresh subagent does not remember round
one, so give it everything:

> Alqa council, round 2. Brief: <brief>. Your round-1 answer: <its answer>. The other advisors
> said: <each other advisor's name + answer, shortened to ~60 words each>. Options: <A-D>.
> Answer in <language>:
> **Rebuttal**: 2-3 sentences. Name at least one advisor you disagree with and say exactly why.
> You may change your mind if someone convinced you; say so.
> **Vote**: one line per option: `<letter>: <score 1-10> - <one-line reason>`.
> **Top choice**: one letter.

Check every reply has a score for every option. If one is missing or malformed, resume or
re-run only that advisor once.

## 4. Count the votes

- For each option: average score (one decimal), lowest and highest score, number of advisors
  whose top choice it is.
- **Decision** = the option with the highest average. If two are within 0.5, say it is a split and
  break the tie by the number of top-choice votes; if still tied, say so plainly and name what
  information would settle it.
- **Dissent**: list every advisor whose top choice is not the decision or who gave the decision
  4 or less, with their one-line reason.
- If the user asked for weights ("Buffett counts double"), apply them and show both the plain and
  the weighted average.

## 5. The report

Write the report in the user's language with these parts:

1. Title, date, the question (one line).
2. **Decision**: the winning option in one or two sentences, and how strong the agreement is.
3. **Score table**: options as rows; one column per advisor; average; top-choice count.
4. **Who disagreed and why** (the dissent list).
5. **Key points**: 4-6 bullets, the strongest arguments from the debate, each with the advisor's name.
6. **The debate**: each advisor's rebuttal, 1-2 lines each, quoting who argued with whom.
7. **Next steps**: 3-5 concrete actions for the next 7-30 days, merged from the advisors' advice.
8. A footer line: "The advisors are AI imitations built from public material, not the real
   people. This is a thinking aid, not professional advice." (translated to the report language)

Save it as `alqa-reports/<YYYY-MM-DD>-<short-latin-slug>.md` in the current project folder
(create the folder).

## 6. The PDF

Make `alqa-reports/<same name>.pdf` from the report with whatever already works on this computer.
Do not install anything without asking the user. Try in this order and stop at the first that works:

1. Write `<same name>.html`: self-contained, UTF-8 (`<meta charset="utf-8">`), clean A4 print
   CSS (`@page { size: A4; margin: 14mm }`), a font that covers Kazakh letters (ә ғ қ ң ө ұ ү һ і),
   e.g. `font-family: "Segoe UI", "Noto Sans", Arial, sans-serif`, the score table as a real
   `<table>`, the decision in a highlighted box. Then print it with a Chromium browser in headless
   mode, for example:
   `"<chrome or msedge path>" --headless=new --disable-gpu --no-pdf-header-footer --print-to-pdf="<abs pdf path>" "file:///<abs html path>"`
   Typical paths: Windows `C:\Program Files\Google\Chrome\Application\chrome.exe` or
   `C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe`; macOS
   `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`; Linux `google-chrome` or `chromium`.
2. `pandoc` if installed (with a PDF engine that handles Unicode, e.g. `--pdf-engine=xelatex` and a
   Unicode font, or `--pdf-engine=weasyprint`).
3. A Python library that is already installed and supports Unicode fonts.

Check the PDF exists and is not empty. If nothing works, keep the HTML, tell the user to open it in
a browser and press Ctrl+P (Cmd+P) → "Save as PDF", or ask whether you may install a tool.

## 7. Reply to the user

In chat, in the user's language: the decision (1-2 sentences), the score table, the dissent in one
or two lines, and the paths of the .md and .pdf files. Keep it short; the details are in the report.

## Rules

- Advisors speak only through their subagents. Do not soften or rewrite their positions; shorten
  only for the round-two summaries.
- Never claim the real people said or endorsed anything. They are imitations.
- Do not search the web unless the user asks; the decision rests on the user's facts.
- For a quick question the user may want a smaller council; 3-4 advisors is fine if they ask.

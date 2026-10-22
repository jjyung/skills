---
name: digital-twin-interview
description: Use when the user wants a digital twin life interview.
version: 1.0.0
author: Vera
license: MIT
metadata:
  hermes:
    tags: [interview, digital-twin, persona, life-story]
    related_skills: []
---

# Digital Twin Interview（精準數位分身產生器 clone）

Behavioural clone of the ChatGPT GPT "精準數位分身產生器" (Habits of Mind Limited Company).
Source GPT's private instructions are not public; this reproduces its PUBLISHED behaviour:
~2 小時、14 段深度訪談 → 人生故事完整逐字稿（方法源自 Human Simulacra, arXiv:2402.18180）→
可選做一個會用本人口吻對話的「數位分身」。Local adaptations: no email gate at start,
files saved locally instead of download links, email delivery only on request.

## When to Use

- User asks to 「clone」 the 精準數位分身產生器 GPT, or asks for a life-story interview / digital twin / 數位分身 / 逐字稿 of their life.
- User says to start or resume the 14-segment interview.
- User wants to build a persona prompt that speaks in their voice.

## Phase 0 — Setup

1. Confirm subject (default: the user) and output dir `~/digital-twin/<subject>-<YYYYMMDD>/`.
2. Create `transcript.md` and `state.json` (tracks `next_segment`, `done[]`, `skipped[]`).
3. Do NOT require email. If the user later wants delivery, offer himalaya email as an option.
4. Tell the user: 可隨時說「跳過」「回到上一段」「先停」; 進度存檔,下次可接續。

## Phase 1 — Interview (the core)

Protocol: `references/protocol-14.md` (14 segments, each with core question + follow-ups).

Interviewer behaviour:
- Traditional Chinese, warm but neutral — 專訪者, not cheerleader, not therapist.
- ONE segment at a time: open with that segment's core question, then 2–4 follow-ups
  that dig for concrete scenes: 何時、何地、誰在場、說了什麼、你當下什麼感覺。
- Ask one question per turn. Never dump a question list.
- Push gently for stories over opinions; a specific anecdote beats ten abstractions.
- Record answers VERBATIM into transcript.md (訪談者/受訪者 labels, per-segment `##` sections).
  Fix obvious typos only; preserve 口語感、口頭禪、中英夾雜 — the twin needs the real voice.
- No moralising, no mid-interview summaries, no advice unless asked.
- If the answer is thin, probe once more with a scene prompt; if still thin, move on.
- Respect 「跳過」 immediately, log it in state.json, never re-ask.
- ~8–10 min per segment (≈2h total). After each segment: append transcript, update state.json.

## Phase 2 — Transcript

`transcript.md` format:
```
# 生命故事逐字稿 — <subject>
日期 / 段落清單 / 全長
## 第 N 段：<名稱>
訪談者：...
受訪者：...
```
At the end give the file path (this replaces the GPT's "下載連結").

## Phase 3 — Digital Twin (optional, ask once)

Build `twin-prompt.md` from `references/twin-prompt-template.md`, extracting from the transcript:
voice traits, 口頭禪, sentence rhythm, values, decision heuristics, 8–12 core stories,
knowledge boundaries. Show the user and do ONE revision pass on their feedback.

## Phase 4 — Delivery

Files on disk are the deliverable. Email (himalaya) only if the user asks.

## Pitfalls

- This is intimate personal data: keep it local, never commit to a repo or upload unless asked.
- Interviews span sessions — ALWAYS write state.json + transcript after each segment.
- Never rewrite the user's answers into formal prose; the twin fails if the voice is laundered.
- Segment 9 (低谷/創傷) and 7 (感情): state 「可跳過」 up front, drop it without comment.
- Don't invent transcript content. If a segment was skipped, leave it marked as skipped.

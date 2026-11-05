# AI chat

> The parts of an AI chat — entry pill, glow, prompt input, messages, thinking, quick replies, a question card and the card for what the AI builds.

- Group: AI
- Lifecycle: wip
- Version: 1.0.0
- Source: `src/components/Ai`
- Also called: AI, assistant, chatbot, copilot, chat input, prompt, composer, message bubble, typing indicator, suggestions, quick replies, AI card, DASH

## Import

```tsx
import { AiPromptInput, AiSuggestionChips, AiReplyChips, AiMessage, AiThinking, AiChoiceCard, AiDraftCard, AiInsight, AiSectionLabel, AiEntryPill, AiGlow } from './components/Ai' // path relative to src/
```

## Usage rules (USAGE.md)

`import { AiPromptInput, AiMessage, AiChoiceCard, … } from './components/Ai'` · Figma: the DASH AI flow (no L3
library components yet) · Docs: `#/ai-chat`

A family of parts for any AI feature — a chat that asks, thinks, answers and builds something (a rule, an indicator,
a screener). Product-neutral: the product's AI mark and name come in through `icon` / `label`, never drawn inside.
Everything else is the normal L3 screen: Actionbar on top, content in the 16 page margin, the input docked at the
bottom.

---

### 1. The parts

| Part | Job |
|---|---|
| `AiEntryPill` | The way into the AI — a small pill in a top bar: the AI mark + its name. |
| `AiGlow` | The AI screen's background: a blurred, slowly turning ring + white wave lines. Decoration only. |
| `AiSectionLabel` | “✦ HERE ARE SOME EXAMPLES ✦” — caps label between two sparkles, above suggestion chips or options. |
| `AiSuggestionChips` | Example prompts in 1–3 rows that scroll sideways together. Tapping one sends it. |
| `AiPromptInput` | The message field at the bottom: a pill with the AI edge and a round Send. |
| `AiMessage` | One turn: `role="user"` (grey pill, right) or `role="assistant"` (plain text, optional bold title). |
| `AiThinking` | The AI is working: three pulsing dots + what it's doing (“Checking past prices…”). |
| `AiReplyChips` | Quick replies under the AI's question; one may be `suggested`. |
| `AiChoiceCard` | The AI asks a question with numbered answers (single choice), optional free-text row and ✕. |
| `AiDraftCard` | What the AI is building: mark + title, then the value so far or the description, then an insight. |
| `AiInsight` | What the AI found, tinted by outcome: `positive` (success), `negative` (error), `neutral` (grey). |

### 2. Screen recipe

- **Start page:** `AiGlow position="center"` behind the content · a short heading · `AiSectionLabel` ·
  `AiSuggestionChips` (full width) · `AiPromptInput` docked at the bottom (16 margin, 16 above the home indicator).
- **Chat page:** `AiGlow position="header"` · Actionbar (back + title) · the chat as a plain `Stack gap="16"` of
  `AiMessage` / `AiThinking` / `AiReplyChips` inside the 16 page margin · `AiPromptInput` docked. When the AI asks a
  question with options, the `AiChoiceCard` takes the input's place (`appear` rises it in); after the answer the input
  comes back.
- **What's being built** (`AiDraftCard`) goes in the Actionbar's `bottom` slot so it stays pinned while the chat
  scrolls; earlier turns under it can be `faded`.
- The glow moves with the flow: `center` on the start page → `header` once the chat starts (it glides, 600 ms).
  `tone="negative"` only when the result is bad news (the insight is `negative`).

### 3. Rules

- **One wrapper per job.** A message is one element — the chat is a `Stack` of messages, no row wrappers. Animate a
  new turn with `appear` (fade up 8; the choice card rises 24), not a wrapper div.
- **Suggestion chips run edge to edge** and bring their own 16 inset — never put them inside the 16 page margin
  (the first chip must sit at 16, not 32). Reply chips wrap and sit inside the margin.
- **One suggested reply at most.** The AI edge marks what the AI recommends; don't use it for anything else.
- **Choice card = single choice.** For several answers, ask again or use checkbox rows in a sheet. Options are short
  (one line). Leave out `onClose` when the question must be answered.
- **Draft card:** `value` while it's being built (a grey chip), `description` once it's done (2 lines; `onOpen` opens
  the full text in a sheet). `insight` unfolds under it when the result arrives.
- **Insight tone is an outcome** (status colours), never profit / loss — even when it's about money. Put amounts in
  `<strong>`; they take the tone's colour.
- **AI accent colours** (`base/hue/ai/*`, the float shadow, the caps tracking) belong to these components only. Don't
  use them on other UI.
- **Entry pill:** one per screen, in the top bar. It hugs its content; the touch area is 48 × 48.

### 4. Accessibility

- Name every control: `AiPromptInput label` (+ `sendLabel`), `AiSuggestionChips label`, `AiReplyChips label`,
  `AiChoiceCard closeLabel`, `freeText.label`, `AiEntryPill ariaLabel`.
- `AiThinking` is `role="status"` — its text is announced. `AiChoiceCard` is a `<section>` named by its question;
  answers are buttons with `aria-pressed` in an ordered list. `AiDraftCard` is an `<article>` named by its title.
- `AiGlow`, the sparkles and the icon slots are decorative (hidden from screen readers).
- With reduced motion: no glow turn, no pulsing, no entrance motion — everything is still shown.

## Overview

### One kit for every AI feature

These parts make up an AI chat that asks, thinks, answers and builds something — a rule, an indicator, a screener. They look the same in every product: the AI mark and its name come from the product (the **icon** and **label** slots), everything else is shared. The rest of the screen is the normal L3 screen — Actionbar on top, content in the 16 margin, the input docked at the bottom.

### Start page and chat page

**Start:** the glow around the middle, a short heading, a sparkle label and example prompts that scroll sideways (edge to edge — the first chip sits at the 16 margin). **Chat:** the glow moves up behind the header; the person's messages are grey pills on the right, the AI's replies are plain text with an optional bold first line. While it works, three dots pulse next to what it's doing. When it needs an answer, a **question card** rises in place of the input; what it's building sits in a **draft card** pinned under the top bar, and its result unfolds under that card as an **insight** strip.

### The AI accent

A thin green edge marks what belongs to the AI — the input, example prompts and the reply it suggests. Use it only on these parts. The insight is an outcome, so it uses the status colours (success, error, grey), never profit / loss. Every part animates in once (fade up, or rise for the question card) and stays still with reduced motion.

Each part takes `className`; `appear` on AiMessage, AiReplyChips and AiChoiceCard plays the entrance, so a chat needs no wrapper divs. AiGlow is an absolutely positioned layer — put it first inside a positioned screen and give the content after it `position: relative`. AI tokens: `--l3-base-hue-ai-*`, `--l3-shadow-ai-float`, `--l3-size-ai-*`, `--l3-letter-spacing-ai-caps`, `--l3-motion-*-ai-*`.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `AiPromptInput` | `{ label; placeholder?; sendLabel?; value?; defaultValue?; onChange?; onSend(text, el); disabled?; autoFocus?; ref? }` |  | The message field: 56 pill, AI edge, float shadow, round primary Send (Button md). Clears itself after a send unless controlled. |
| `AiSuggestionChips` | `{ rows: string[][]; onPick(prompt, el); label }` |  | Example prompts in rows that scroll sideways together; 36-tall chips with the AI edge. Full width, own 16 inset. |
| `AiReplyChips` | `{ options: { label; onClick?; suggested? }[]; label; appear? }` |  | Quick replies that wrap; the suggested one has the AI edge and shadow-sm. |
| `AiMessage` | `{ role: 'user' \| 'assistant'; children; title? (assistant); faded?; appear? }` |  | User: grey pill on the right (max 80%, Label/14). Assistant: no bubble, optional Heading/14 title + Description/14 text. One element. |
| `AiThinking` | `{ children; icon? }` |  | Three pulsing dots + what the AI is doing; role="status". |
| `AiChoiceCard` | `{ question; options: string[]; selected?; onAnswer(answer); freeText?: { placeholder; label }; onClose?; closeLabel?; headingLevel?; appear? }` |  | A question with numbered single-choice answers (aria-pressed buttons in an <ol>), filled tick on the pick, optional free-text row and ✕. Radius 16, padding 12, AI float. |
| `AiDraftCard` | `{ icon?; title; value \| description (+ onOpen?, openLabel?); insight?; headingLevel? }` |  | What the AI builds: mark (32 slot) + title, dashed hairline, then the value chip or a 2-line description (a button with a chevron when onOpen); insight unfolds under it. |
| `AiInsight` | `{ tone: 'positive' \| 'negative' \| 'neutral'; icon; children; shape?: 'strip' \| 'card' }` |  | Tinted result: success / error / grey; <strong> amounts take the tone. strip = edge to edge in a draft card; card = radius 8 with a tone border. |
| `AiSectionLabel` | `{ children; as?: 'h2' \| 'h3' \| 'h4' \| 'p' }` |  | Caps Label/12 with tracking between two sparkles. |
| `AiEntryPill` | `{ icon; label; ariaLabel; onClick? }` |  | The way into the AI (Figma "Dash entry point"): mark + name on a white-into-brand pill with a green edge. Hugs; 48 × 48 touch area. |
| `AiGlow` | `{ position?: 'header' \| 'center' \| 'bottom'; tone?: 'brand' \| 'negative' }` | `'header', 'brand'` | Decorative background layer (absolute, inset 0, aria-hidden): a blurred turning ring + wave lines. Glides when position changes. |

## Tokens used

- `base/hue/ai/edge · glow-1 · glow-2 · glow-3 (AI accent)`
- `shadow/ai-float`
- `size/ai-chip · ai-glow-blur`
- `letter-spacing/ai-caps`
- `motion/duration/ai-pin · ai-pop · ai-glow-move · ai-glow-turn`
- `motion/easing/ai-pop`
- `surface/default · secondary`
- `border/light`
- `surface · content · border/accent/success · error (insight)`
- `Label/12 · Label/14 · Heading/14 · Description/14`

## Recent changes

- **1.0.0** (2026-10-23) New: AI chat components: AI chat: a kit for any AI feature, taken from the DASH AI flow and made product-neutral (the AI mark and name come from the product). Entry pill · glow background (header / centre / bottom, brand or negative, slowly turning) · sparkle section label · example prompts in rows that scroll sideways · prompt input with a round Send · messages (person: grey pill on the right; AI: plain text with an optional bold title) · thinking dots · quick replies (one suggested) · question card with numbered answers, a free-text row and ✕ · draft card for what the AI builds, with an insight strip that unfolds under it. AI accent tokens: base/hue/ai (edge, glow 1–3), shadow/ai-float, size/ai-chip and ai-glow-blur, letter-spacing/ai-caps, motion ai-pin · ai-pop · ai-glow-move · ai-glow-turn and easing ai-pop. For these components only. Every part takes className; appear on AiMessage, AiReplyChips (fade up 8) and AiChoiceCard (rise 24) plays the entrance, so apps need no wrapper divs. Reduced motion turns off the glow turn, the dots and all entrances. Every control is named (label / sendLabel / closeLabel / freeText.label / ariaLabel). AiThinking is role="status"; AiChoiceCard is a <section> named by its question, answers are aria-pressed buttons in an <ol>; AiDraftCard is an <article> named by its title; the glow, sparkles and icon slots are decorative.

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

A single-file, German-language learning app for SQL/database vocabulary: `sql-begriffe/index.html` (one "mini" among several; the root `index.html` is the bi_minis tile overview with a hand-maintained `projects` array). There is no build system, package manager, tests, linter, or git repo. Open the file directly in a browser to run it; edit it in place.

A second mini, `scratch-bloecke/index.html`, quizzes Scratch 3 blocks (Bewegung, Aussehen, Ereignisse, Steuerung, Fühlen, Operatoren, Variablen). Blocks are rendered with the original Scratch colors/shapes from the library `L` (`def(...)`), instances are built with `K(id, ...values)`; tasks are added with `pick/cmp/pred/build/bcat`. Task counts are computed, nothing is hardcoded.

## Architecture

Everything (CSS, markup, JS) lives in the one HTML file, with no external dependencies.

- **Data** is two arrays at the top of the `<script>`:
  - `terms` — `{term, category, explanation, example}`. `example` is an SQL snippet (newlines written as `\n`) shown under the explanation. Drives the training "Begriffs-Wand" and the term buttons in the test.
  - `questions` — `{q, a:[...], ex}`. `ex` is an SQL snippet shown in the feedback after checking. Each entry in `a` must match a `term` string exactly. Questions can have several correct answers, and the user must select all of them.
- **Two modes** toggled by `setMode()`: `trainingView` (click a term to read its explanation) and `testView` (shuffled questions, select term buttons, then "Antwort prüfen"). The test starts lazily on first entry to test mode.
- Term and question order is shuffled on each start (`shuffle`). The UI is built with DOM APIs and `textContent`, not string-built HTML.
- Test state is module-level `let` variables (`testQuestions`, `questionIndex`, `selected`, `checked`, `correct`, `answered`), reset by `startTest()`.
- **Hardcoded "19"**: the question count appears in the markup ("FRAGE 1 VON 19", "Bearbeitet: …/19", `finalScore` "0/19"). If you add or remove entries in `questions`, check that these still match the array length.

## Conventions

- UI text is German, and the file uses real umlauts (ä, ö, ü, ß). Keep new terms, questions, and categories in German.
- Theming goes through CSS variables in `:root` (dark theme). Reuse them rather than adding new hex colors.
- Adding a term means adding it to `terms` with one of the existing `category` values (or a new one), and optionally a question in `questions` that references it.

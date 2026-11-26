# Elfadela Institute (معهد الفضيلة) — study web app

Static site for Islamic studies (Fiqh, Aqida, Quranic Sciences): lecture recordings, book PDFs, and chapter quizzes.
Hosted on GitHub Pages. No build step, no backend.

## Structure
- `index.html` — the whole app (HTML + CSS + JS in one file)
- `media/` — compressed lecture recordings (`.mp4`, audio only) and PDFs (books, reviews)

## Where things live in index.html
- `T` — UI strings, `en` and `ar`. Every new label needs both.
- `fiqhChapters`, `quranLectures`, `aqidaLectures` — subject items. Item fields:
  `ar`, `en`, `n` (badge number), `plain` (no "باب" prefix), `soon` (coming soon), `recs` (`[{src, mins}]`), `quiz`.
- Quiz arrays (`watersQuiz`, `asarQuiz`, `najasatQuiz`, `aniyaQuiz`, `qadaQuiz`, `fitraQuiz`) — `{q, o:[options], a:<index of correct option>}`. `TF` = ["صح","خطأ"].
- `SUBJ` — maps subject key → items, `total` (lecture count), `docs` (resource keys).
- `DOCS` — resources (book / review PDFs) shown under the Resources tab.
- Routes are hash-based: `#fiqh`, `#fiqh-2`, `#fiqh-2-quiz`, `#quran-3`, `#aqida-1`, `#<doc-key>` (PDF reader).

## Rules from the owner (Shaimaa) — follow these
- Copy quiz questions and answers EXACTLY as written in screenshots. Never fix spelling or wording without asking first; list suspected typos and wait for confirmation.
- Ask clarifying questions before building anything new.
- When offering design/style options, show visual previews before applying.
- Keep replies concise and direct.
- Default language is Arabic (RTL); English switches the layout to LTR. A language the student picked is remembered on their device. Quiz content always stays Arabic.
- Subject pages open on the "المحاضرات / Lectures" tab every time (don't remember the last tab).

## Adding media
- Recordings: compress to mono AAC in `.mp4`, keep each file under ~20 MB:
  `ffmpeg -i input.m4a -vn -ac 1 -c:a aac -b:a 48k -movflags +faststart media/name.mp4`
  (for files over ~55 min lower the bitrate, e.g. `-b:a 36k`; for ~1h45 use `-ar 16000 -b:a 24k`)
- Then add `{src:"media/name.mp4", mins:N}` to the item's `recs`.
- PDFs go in `media/` and get an entry in `DOCS` + the subject's `docs` list.

## Check before pushing
- Open `index.html` via a local server (`python3 -m http.server`) and test: menu, both languages, a recording, a quiz, the PDF reader, mobile width (~390px).
- Accessibility: tap targets ≥ 44px on mobile, visible focus, contrast ≥ 4.5:1.

## Deploy
```
git add .
git commit -m "Describe the change"
git push
```
GitHub Pages updates within 1–2 minutes.

# YouTube Student: a student-reviewed, low-data learning prototype

A single-file prototype (`index.html`) that shows how YouTube could help students find the *right* teacher and keep learning even on a weak or expensive internet connection.

It has two portals in one page:

- **YouTube**: a familiar video site, ranked by reviews (or views, for comparison).
- **YouTube Student**: a low-data study mode with notes + audio lessons, quizzes, verified student reviews and an offline mode.

> This is a demo. All videos, teachers, reviews and lessons are hardcoded sample data. No real video is streamed.

---

## Why this project

Two problems students face on YouTube today:

1. **Finding the right lecture is confusing.** Many videos cover one topic, and view count does not tell you which teacher explains it best.
2. **Data is expensive.** Long, high-quality videos use a lot of mobile data.

**Idea:** a Student-Reviewed, Low-Data Learning System.

- Teachers share notes with an audio explanation, so a lesson costs a few MB instead of a few hundred.
- Students who have passed a short quiz review the lesson, and those reviews drive the ranking, not just views.

**Deliberately not built:** a separate learning platform, live classes, or an AI tutor. The aim is to improve a platform students already use.

---

## How to run

1. Put `index.html` anywhere and open it in a modern browser (Chrome, Edge, Firefox, Safari).
2. No build step, no server, no install.

Notes:
- The heading font (Fraunces) loads from Google Fonts. Without internet the page falls back to Georgia.
- Audio uses the browser's built-in text-to-speech. Hindi audio needs a Hindi voice installed on the device.
- File downloads work in a normal browser tab. Some preview/iframe environments block downloads.

---

## Demo logins

| Student ID | Name | Level |
|---|---|---|
| `vips@himanshi` | Himanshi | Boards |
| `vips@chayan` | Chayan | Boards |
| `STU1001` | Aarav | IIT-JEE |
| `STU1002` | Diya | Boards |
| `STU1003` | Kabir | Olympiad |

IDs are not case sensitive. You can also create your own account (see [Student login](#student-login-and-accounts)).

---

## Features

### 1. YouTube portal (the "normal" side)

- Video grid with a generated banner per video (topic colour + emoji). To use a real image, add `img:'https://...'` to a video object.
- **Live search** by title or teacher, updating as you type.
- **Category chips:** All, Maths, Physics, Biology.
- **Sorting:**
  - ⭐ *Best student reviews* (default): ranked by weighted review score.
  - 🔥 *Most viewed*: ranked by views only, so you can see how the two orders differ.
- Hover effects: cards lift and a ▶ icon appears.
- **Watch page:**
  - Video quality selector and live data estimate.
  - Download quality for video and audio.
  - 👍 Like and Subscribe toggles (saved in the browser). Subscribed teachers get a ✓ on the home grid.
  - *Recommended by student reviews* list on the side. Videos of the same topic come first, then higher review score.
  - Shortcut button to switch to YouTube Student for the same topic.

### 2. YouTube Student portal

- **Topics:** Bayes theorem (Maths), Newton's laws (Physics), Photosynthesis (Biology), Derivatives (Maths). Each topic has 4 to 5 lectures from different teachers.
- **Home:** topic cards, a "Top rated by students" strip, and a "Best quiz" badge on topics you have attempted.
- **Levels:** Boards, IIT-JEE and Olympiad. The list can be filtered by level, and a logged-in student gets their own level marked *Recommended*.
- **Ranked lecture list:** every lecture of a topic, from all teachers, sorted by learning score (ties broken by number of reviews). The top lecture gets a 🏆 *Top pick* badge.
- **Best teachers for this topic:** teachers ranked by the weighted score of their lectures.
- **Lesson page (notes + audio):**
  - Five slides per topic with a formula box where relevant.
  - Play / Pause audio, Previous / Next slide. The audio follows the slides.
  - **Data meter:** compares what the video would use with what notes + audio uses.
  - **Summary** per level, an **"I still don't get it"** simple explanation, and an **Ask a doubt** box (keyword-based answers from the saved notes).
- **Back button** on every inner screen, plus a quiz/login return path that brings you back to the same lesson and slide.

### 3. Languages (Hindi and English)

- **Notes language** and **audio language** are chosen separately, for example Hindi notes with English audio.
- Hindi covers slides, summaries, the simple explanation and the downloaded notes for all four topics.
- The choice is remembered on the device.
- Not translated yet: quiz questions and doubt-box answers (English only).

### 4. Quizzes

- Three multiple-choice questions per topic, available offline.
- After submitting, you see your score and the correct answers for what you missed.
- Your **best score per topic** is saved on the device and shown on the topic card and profile.
- The quiz score also decides how much your review counts (see below).

### 5. Review system

**Learning score**

```
learning score = Σ(rating × weight) / Σ(weight)
weight         = 0.5 + quiz answers correct   (range 0.5 to 3.5)
```

- A student who understood the lesson (high quiz score) counts more than one who guessed.
- Views are **not** used in this score.
- Only **verified, logged-in students** can review. One review per student per lesson.
- After a review, the lesson shows how its rank moved (for example "from #3 to #1").
- In this demo, new reviews update rankings during the session only. They reset on reload.

**Comments**

- Each lesson has a comment box, available to logged-in students only. Comments are saved on the device.
- Logged-out users see only a locked message with a login button. No review or comment box is shown.

### 6. Student login and accounts

- **Log in** with a Student ID.
- **Create account** with: full name, age, institution name, admission / enrollment number and target level.
  - The form validates all fields and blocks a second account for the same enrollment number.
  - A new Student ID (like `STU1004`) is generated, and the student is logged in after confirming.
- **Profile page** (click your name in the header): institution, age, enrollment number, target level, review and comment counts, best quiz scores, saved offline items, and language choices.
- Accounts are stored in the browser's `localStorage` only. A real product would verify the enrollment number against school records.

### 7. Offline mode

- A toggle switch in the Student header simulates losing internet.
- While offline:
  - Saved lessons (offline packs), summaries and quizzes still work.
  - Lessons that were not saved are dimmed and cannot be opened.
  - Downloads are blocked.
  - Reviews are queued on the device, and **sync automatically** when you switch back online.

### 8. Video and audio quality, and downloads

| Option | Choices |
|---|---|
| Video playback quality | 144p, 360p, 480p, 720p, 1080p |
| Video download quality | 144p, 360p, 480p, 720p, 1080p |
| Audio download quality | Low, Standard, High |

- Sizes are scaled from each video's 720p size, and shown in MB before you tap.
- **Download types:** video, audio only, and **offline pack** (notes + audio + summary + quiz).
- The same video can be saved at several qualities.
- **Downloads page:** shows each item with its quality and size, the total space used, and Remove.
- **Player for downloaded items:** Play / Pause, Stop, ±10 second skip, a seek bar and speed (0.75× to 2×). It is a simulated player: it runs a timer, not real media.
- Downloaded items are remembered between visits.

### 9. Notes download (a real file)

Every notes download button creates a real, styled **`.html` notes file**:

- Lesson page: *Download notes file*
- YouTube watch page: *Notes file*
- Downloads page: *Notes file* next to each offline pack. Saving an offline pack also downloads it.

The file contains the title, teacher, level, numbered slides with formulas, key points, the simple explanation and practice questions with answers. It is written in the chosen notes language and has a *Print / Save as PDF* button. It opens on any device without internet.

### 10. Look and feel

- YouTube Student has its own theme: gradient background, coloured topic cards, hover lifts, slide animations, button press effects and a blurred sticky header.
- Animations respect the system's "reduce motion" setting.
- The layout is responsive for phones.

---

## Data stored on the device

| `localStorage` key | What it holds |
|---|---|
| `ys` | Downloaded items |
| `ysq` | Best quiz score per topic |
| `ysc` | Comments |
| `ysl` | Notes and audio language |
| `ysa` | Accounts created on this device |
| `ysx` | Likes and subscriptions |

Clearing the browser's site data resets all of these.

---

## Customising the prototype

All data sits at the top of the script in `index.html`.

- **Add a student:** add an entry to `STUDENTS` (`name`, `cls`, `goal`).
- **Add a video:** add a row to the list that feeds `videos.push(...)`, with topic, title, teacher, views, video MB, notes MB, level, review count, average rating and review weight.
- **Add a topic:** add an entry to `T` (name, emoji, subject, slides, quiz, summary per level, simple explanation, doubt answers), add its `.hi` Hindi block, and add its colours to `GR`.
- **Use real thumbnails:** set `img` on a video.

---

## Known limitations

- All content is sample data. Videos are not real and the audio is machine-read, not teacher-recorded.
- Review changes are not saved between page loads.
- Downloads and the player are simulated. Sizes are estimates.
- The doubt box matches keywords. It is not an AI tutor.
- Login is a demo. There is no password and no real school verification.
- Quiz questions and doubt answers are English only.

## Ideas for the next version

- Teacher studio: upload notes + audio and preview that they match before publishing.
- Review tags (clear explanation, useful notes, audio matched notes) alongside stars.
- Protection against fake or unfair reviews beyond the one-review limit and quiz weighting.
- Real recorded audio, real downloads and a backend for accounts, reviews and comments.
- More topics, subjects and languages.

# Muscle Quiz: Stat! 💪

A gamified, single-file study game for **46 anatomy terms** covering **muscle tissue, the muscles of the head, eyes, chest, abdomen & back, and their connective-tissue landmarks**.

**▶ Play it:** https://producer456hub.github.io/muscle-quiz/

Built in the same engine as [code-crew](https://github.com/producer456hub/code-crew), reskinned for a muscles unit (no EMT/ECG/phlebotomy framing).

## How to play
Pick a tile from the 6-category board (a fresh **random board each game** is drawn from a **92-question bank — two questions per term**, one on *structure* and one on *action/function*) and answer before the timer runs out (the timer is optional — toggle **TIMED MODE** off any time). Each question is multiple-choice with four options.

- 🔥 **Combos** — consecutive correct answers raise your multiplier (up to ×3).
- ⏱️ **Speed bonus** — faster answers score more.
- ⚡ **Bonus tiles** — two random tiles each board pay **double points**.
- ❤️ **Patient HP** — right answers heal the patient, wrong ones hurt them (scaled by tile value). Two pixel medic-bots react, banter, drop muscle facts, and deliver a **diagnosis** at the end.
- 🧠 **Adaptive** — terms you miss (or haven't seen) resurface more often until you nail them.
- 🏅 **Ranks & 🏆 leaderboard** — finish to earn a rank, review missed terms, and add your name to the high-score board.

## Categories (46 terms)
| Category | Terms |
|---|---|
| **Muscle Tissue** | skeletal muscle fiber, myofibrils, sarcomere, sarcolemma, sarcoplasm, T-tubules, sarcoplasmic reticulum, nucleus, mitochondria |
| **Facial Muscles** | occipitalis, frontalis, orbicularis oculi, nasalis, zygomaticus major/minor, masseter, buccinator, orbicularis oris, depressor anguli oris, risorius, mentalis, temporalis |
| **Eye Muscles** | inferior/superior oblique, medial/lateral/superior/inferior rectus |
| **Chest & Breathing** | pectoralis major/minor, diaphragm, external/internal/innermost intercostals, serratus anterior |
| **Abdomen & Back** | external/internal oblique, transverse abdominis, rectus abdominis, latissimus dorsi |
| **Neck & Landmarks** | sternocleidomastoid, trapezius, anterior rectus sheath, tendinous intersections, linea alba, galea aponeurotica |

### A note on O/I/A
The 7 muscles your sheet marks **O/I/A** — sternocleidomastoid, trapezius, pectoralis major, pectoralis minor, rectus abdominis, latissimus dorsi — include **origin, insertion, and primary action** in their definitions. These use standard anatomy values; **double-check them against the Week 8 PowerPoint**, since that's your source of truth, and tell me if any differ.

## High-score leaderboard
This game points at a **`muscle_scores`** table in the same Supabase project code-crew uses (so the two games keep **separate** boards). Until that table exists, scores fall back to **this device** (localStorage) automatically.

To turn on the global board, run this once in the Supabase project's **SQL editor**:

```sql
create table muscle_scores (
  id bigint generated always as identity primary key,
  name text not null,
  score int not null,
  correct int not null,
  created_at timestamptz default now()
);
alter table muscle_scores enable row level security;
-- allow anyone to read the board:
create policy "public read"  on muscle_scores for select using (true);
-- allow anyone to add a score:
create policy "public insert" on muscle_scores for insert with check (
  char_length(name) <= 16 and score >= 0 and score < 100000 and correct between 0 and 30
);
```

The Supabase URL and **anon (publishable)** key are already wired in `index.html` — both are safe to expose.

## Files
- `index.html` — the entire game (HTML, CSS, JS, pixel-art sprites, chiptune sound — no build step, no dependencies)
- `README.md` — this file

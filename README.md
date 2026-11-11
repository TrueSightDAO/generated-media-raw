# generated-media-raw

Archive of **generated / compiled video deliverables** — the films, cuts and animatics we
produce from archive footage. This is **NOT** raw farm capture.

| | Repo | Holds |
|---|---|---|
| Raw capture | `TrueSightDAO/farm-media-raw` | original HEIC/MOV from farm visits |
| **Generated** | **`TrueSightDAO/generated-media-raw`** ← you are here | compiled videos + their lineage |
| Manifests (raw) | `TrueSightDAO/farm_media_manifests` | sha256, GPS, objects, `yt_id` |
| Pipeline doc | `agentic_ai_context/MEDIA_ARCHIVE_PIPELINE.md` (MAP) | intake → process → archive → manifest |

## Why this repo exists

MAP indexes **raw** footage. It has no vocabulary for the *inverse* edge: **which raw clips a
generated video consumed.** Without that edge, the same establishing shot gets re-cut into
every film and nobody notices — the archive slowly turns into the same 6 clips on rotation.

Each manifest therefore records, per beat: the source `sha256`, the in/out timecode, and a
**confidence** level.

## The honesty rule

> An unverified beat is written as `"confidence": "unknown"` **with the search method that
> failed.** Never guess a source, and never leave the field blank.

A blank field is indistinguishable from "nobody looked." Recording *"swept all 12 raw MOVs at
fps=1/3, best correlation 0.31 = noise"* tells the next agent the work was done and to try a
different approach — not to repeat it.

## Schema

```json
{
  "entity_type": "generated",
  "generated_id": "<slug>",
  "version": "v9",
  "artifact": { "sha256": "…", "duration_seconds": 12.542, "frames": 301 },
  "youtube": { "yt_id": "…", "privacy": "public", "marked_generated": true },
  "lineage": [
    { "beat": 1, "label": "…", "in": 0.0, "out": 3.0,
      "source_sha256": "…", "in_point": 0.0, "out_point": 3.0,
      "confidence": "verified | unknown" }
  ],
  "clearance_flags": ["…"],
  "reuse_guard": { "reserved_sources": ["…"] }
}
```

## Reuse guard — read before compiling

Query every manifest in `generated_video_manifests/` first. If a source beat already appears,
cast a **different** clip in that role. `reuse_guard.reserved_sources` lists sources already
spent.

## Placement rule

- **Manifests** → this repo, `generated_video_manifests/<slug>.json` (small, diffable, committed)
- **Video blobs** → **YouTube** (public), referenced by `yt_id`. Never commit large blobs; never
  copy blobs into `farm-media-raw`.
- **Raw source** → stays in `farm-media-raw`; link by `sha256`, never duplicate.

## Marking convention for uploads

Every generated upload is prefixed **`[GENERATED]`** in its title and states in the description
that it is compiled rather than raw capture, so a viewer or a downstream licensee cannot mistake
it for original footage.

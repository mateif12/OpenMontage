# D-DAY: THE MORNING THAT CHANGED HISTORY — Production Plan & Storyboard

Status: **PRE-PRODUCTION — awaiting provider decision (no assets generated, no money spent)**
Deliverable: YouTube Short, MP4 / H.264 + AAC, 1080x1920 (9:16), 30 fps, ≤ 55.0 s

---

## 1. Capability audit (inspected 2026-10-30)

### 1.1 What this repository actually contains

| README claims | Present in this checkout? |
|---|---|
| `tools/` (video/image/TTS/music providers, compose, stitch) | **No** |
| `pipeline_defs/` (YAML pipelines, e.g. cinematic, documentary montage) | **No** |
| `skills/` (stage director skills) | **No** |
| `remotion-composer/` (render engine) | **No** |
| `Makefile` (`make setup`) | **No** |
| `lib/` helpers (scoring, shot_prompt_builder, media_profiles, slideshow_risk, checkpoint, corpus…) | Yes (3,741 lines) |

The `lib/` modules import `tools.*`/`pipeline_defs/` that don't exist, so the pipeline can't run as shipped.
**Not one of the 14 video-generation providers listed in the README (Kling, Veo, Runway, WAN, LTX, etc.) is implemented in this checkout.**
`lib/media_profiles.py` does define a usable `youtube_shorts` profile: 1080x1920, 30 fps, libx264, AAC, CRF 20.

> ⚠️ **Security note:** this fork has one content commit, made by an account unrelated to the repo owner. Its README tells users to download and run prebuilt installers (`.exe`/`.run`/`.dmg`) from "releases", but the source code those installers would be built from is missing. That pattern is common in repo-clone malware campaigns. **Don't run any binaries from this fork's releases.** Get OpenMontage from its official upstream repository instead.

### 1.2 What this cloud container can do

| Resource | Status |
|---|---|
| GPU | **None** (4 vCPU, 15 GB RAM, no PyTorch). Local AI video models (WAN 2.1, Hunyuan, CogVideo, LTX-Video) can't run here. |
| API keys | **None configured** (no fal.ai / Kling / Runway / Google / OpenAI / ElevenLabs / Pexels keys) |
| FFmpeg / ffprobe | Available. Composition, grading, subtitles, mixing and export can all be done here. |
| Node 22 + npm registry | Available (Remotion/HyperFrames can be installed if wanted) |
| Network: PyPI, npm, GitHub | Allowed |
| Network: archive.org, commons/upload.wikimedia.org, huggingface.co, images.pexels.com, fal.run, api.openverse.org, nara.gov | **Blocked by the environment's network policy** |

### 1.3 Conclusion

**Photorealistic AI video can't be produced in this environment at zero cost.** It needs **one** of:

| Route | Provider / requirement | Cost | Notes |
|---|---|---|---|
| A. Cloud AI video (recommended for the brief as written) | **Kling** (e.g. via fal.ai), **Google Veo 3** (via fal.ai or Google), or **Runway Gen-4**. Needs an API key plus the provider host allowed in network settings. | **Paid.** Per-second pricing that changes often; I will quote from the provider's live pricing page before generating. (The README reports $1.33 for six Kling clips in one demo. That figure is unverified and not a quote.) | Best realism. Providers may refuse or soften combat prompts; storyboard is written non-graphic to reduce this. |
| B. Local open-source AI video | WAN 2.1 / LTX-Video / Hunyuan on **your own NVIDIA GPU machine** | Free (your hardware) | Not runnable in this container. Realism is noticeably below route A, and per-clip render times are long on consumer GPUs. |
| C. **Free alternative: authentic archival footage** | Real 1944 footage: US Army Signal Corps / US Coast Guard / Library & Archives Canada / IWM material on archive.org and Wikimedia Commons. Needs `archive.org`, `commons.wikimedia.org` and `upload.wikimedia.org` allowed in network settings. | Free | Most historically accurate option. US federal works are generally public domain, but **rights must be checked clip by clip** (IWM/Canadian material may carry other terms). Visuals would be black-and-white archival, not AI reconstruction, labelled "ARCHIVAL FOOTAGE". |
| D. Hybrid | Route C archival + a few route A AI shots (e.g. hook aerial, cemetery sunset) | Low paid | AI shots carry an on-screen "AI RECONSTRUCTION" tag. Archival shots carry an "ARCHIVAL FOOTAGE · 1944" tag. |

**Narration (deep male documentary voice):**
- **Free:** Piper TTS. Its voice models download from huggingface.co, which is blocked here, so that host must be allowed.
- **Paid:** ElevenLabs or Google TTS for higher quality.
- **Free:** you record it yourself.

**Music/SFX (free):** CC0 / CC-BY orchestral and SFX libraries (e.g. Freesound, Incompetech, YouTube Audio Library). All are external downloads, so they need either a network allowance or you uploading the files.

---

## 2. Historical fact-check of the brief

| Brief item | Verdict | Action |
|---|---|---|
| "Over 156,000 Allied troops" | Consistent with the widely cited figure of ~156,000 (often given as 156,115) landed by sea **and air** on 6 June 1944. | Keep. Note that ~24,000 of them were airborne, so "crossed the English Channel" holds (by sea or air). |
| Hook visual: hundreds of landing craft at **dawn** | The fleet is accurate. The morning, however, was **overcast with rough seas** (the weather that forced the one-day postponement). US H-Hour was 06:30. | Use **grey, overcast dawn light with choppy sea**, not a golden sunrise. |
| Scene 3: Omaha, devastating resistance, many never got past the shoreline | Accurate (US 1st & 29th Infantry Divisions; heaviest losses of the five beaches). | Keep. Non-graphic depiction. |
| Scene 4: tanks moving through smoke | Accurate in general. At Omaha most DD (amphibious) Shermans launched offshore foundered. | Show M4 Shermans **without** implying a specific beach, or show British/Canadian armour. |
| Scene 5: troops move inland through Normandy villages "by end of D-Day" | Partly accurate. The Omaha beachhead was shallow on 6 June. Deeper advances were made by Canadian/British forces at Juno/Gold/Sword and by US airborne troops (e.g. Sainte-Mère-Église). | Show **British/Canadian** troops (Mk II/III helmets, battledress) in a Normandy village. Don't caption it as Omaha. |
| Scene 6: "Rows of white crosses", Normandy American Cemetery | The headstones are white marble **Latin crosses and Stars of David**. The cemetery sits at Colleville-sur-Mer overlooking Omaha Beach. | Include Stars of David among the crosses. |
| "LEST WE FORGET" | A Commonwealth remembrance phrase (Kipling, *Recessional*), shown here over a US cemetery. Not wrong, given the multinational Allied force. | Keep as requested. Alternative: "WE WILL NEVER FORGET", which mirrors the narration. **Your call.** |
| "Do not invent events" | No numbers or events beyond the brief are added. | — |

---

## 3. Look bible (consistency across all shots)

- **Lens/camera:** 35 mm and 50 mm equivalents; shallow depth of field on close-ups; handheld micro-shake in combat scenes; stabilised drone/crane for scenes 1, 5 and 6.
- **Grade:** desaturated (≈ −35% saturation), teal-grey shadows, warm-neutral skin, lifted blacks, fine 35 mm grain. The sunset in scene 6 is the only warm, saturated moment, as an emotional release.
- **Atmosphere:** sea spray, smoke, overcast sky, barrage balloons over the fleet (period-accurate).
- **US troops (Omaha):** M1 helmet, often with netting; olive-drab wool or HBT uniform; M1926 inflatable life belt; assault gas-mask bag; M1 Garand, BAR, M1 carbine; medics with Red Cross brassard and helmet markings.
- **British/Canadian:** Mk II or Mk III "turtle" helmet; khaki battledress; 1937-pattern webbing; Lee-Enfield No. 4, Bren gun, Sten.
- **Craft & armour:** LCVP "Higgins boat" (plywood hull, steel bow ramp) for US troops; LCA for British/Canadian troops; M4 Sherman.
- **German obstacles:** Czech hedgehogs, Belgian gates, wooden stakes topped with Teller mines, concrete strongpoints on bluffs.
- **Global negative prompt:** `modern weapons, modern vehicles, kevlar, plate carriers, digital camo, M16, AK-47, Humvee, helicopters, drones, jet aircraft, modern buildings, wind turbines, text, watermark, logos, gore, dismemberment, blood spray, swastika close-ups, cartoon, CGI look, oversaturated`
- **Disclosure:** persistent corner tag **"AI RECONSTRUCTION"** on every generated shot (and "ARCHIVAL FOOTAGE · 1944" on any real footage), plus YouTube's *altered or synthetic content* disclosure checked at upload.

---

## 4. Timeline overview (55.0 s total, 1,650 frames @ 30 fps)

| # | Scene | In–Out (s) | Frames | Shots | On-screen text | Est. VO length* |
|---|---|---|---|---|---|---|
| 1 | The Hook | 0.0–6.0 | 0–179 | 2 | JUNE 6, 1944 | ~5.6 s |
| 2 | The Invasion | 6.0–15.0 | 180–449 | 3 | 156,000+ ALLIED TROOPS | ~6.5 s |
| 3 | Omaha Beach | 15.0–25.0 | 450–749 | 3 | OMAHA BEACH | ~6.2 s |
| 4 | The Battle | 25.0–35.0 | 750–1049 | 3 | COURAGE UNDER FIRE | ~4.6 s |
| 5 | The Turning Point | 35.0–46.0 | 1050–1379 | 2 | THE LIBERATION HAD BEGUN | ~7.7 s |
| 6 | The Emotional Ending | 46.0–55.0 | 1380–1649 | 2 | LEST WE FORGET | ~7.7 s |

*Estimates at ~155 wpm with numbers spoken in full. Total VO ≈ 38 s, so it fits with breathing room. Scene cut points are re-timed to the actual recorded narration.

**Total generated clips:** 15 shots. Each is generated at 5 s (trimmed in the edit) and requested at 9:16 natively. If a provider only outputs 16:9, the shot is generated wide and centre-cropped, with framing kept centre-safe.

---

## 5. Storyboard

Safe zones for every shot: keep faces and key action in the centre 1080x1300 area. Keep the bottom 380 px clear for Shorts UI and subtitles, and the top 200 px clear for the title and date tags.

### SCENE 1 — THE HOOK (0.0–6.0 s)
**Narration (starts at 0.15 s):** "June 6th, 1944. One morning would change the fate of the entire world."
**Text:** **JUNE 6, 1944**. Slams in at frame 0 with a bass hit; scale 115%→100% over 8 frames; gold accent.

| Shot | Time | Visual | Camera | AI prompt (abridged) |
|---|---|---|---|---|
| 1A | 0.0–3.2 | Hundreds of landing craft and warships cutting white wakes toward a grey coastline; barrage balloons overhead | High aerial, fast push forward and down | "Aerial view of hundreds of WWII Allied landing craft and warships crossing a choppy grey English Channel toward the Normandy coast at overcast dawn, 6 June 1944, white wakes, barrage balloons, mist on the horizon, photorealistic, desaturated 1940s colour, 35mm film grain, vertical 9:16" |
| 1B | 3.2–6.0 | Low over the water past an LCVP bow ramp; the coast emerges through haze; distant flashes from the naval bombardment | Low tracking, slight roll | "Low tracking shot skimming over rough sea alongside a WWII LCVP Higgins boat, steel bow ramp, spray, distant battleship gun flashes through haze, overcast dawn, cinematic, photorealistic" |

**Audio:** sub-bass impact at frame 0; strings swell; wind and sea.
**Transition:** whip-pan into scene 2.

### SCENE 2 — THE INVASION (6.0–15.0 s)
**Narration:** "Over 156,000 Allied troops crossed the English Channel to invade Nazi-occupied France."
**Text:** **156,000+ ALLIED TROOPS**. Number counts up 0 → 156,000 over 20 frames (synced to "one hundred fifty-six thousand"), then "+".

| Shot | Time | Visual | Camera | AI prompt (abridged) |
|---|---|---|---|---|
| 2A | 6.0–9.0 | Inside an LCVP: rows of US infantry, M1 helmets, life belts, spray crashing over the sides | Handheld, from the rear of the boat looking toward the ramp | "Inside a WWII LCVP Higgins boat, rows of American soldiers in 1944 olive-drab uniforms, M1 helmets with netting, inflatable life belts, M1 Garand rifles, waves crashing over the plywood sides, grey overcast light, handheld, photorealistic" |
| 2B | 9.0–12.0 | Close-up of a young soldier: wet face, jaw set, fear and resolve; lips moving in prayer (no audible dialogue) | 50 mm close-up, shallow DOF | "Close-up of a young American WWII soldier, 1944, wet face, M1 helmet chin strap, scared but determined eyes, sea spray, shallow depth of field, desaturated cinematic grade" |
| 2C | 12.0–15.0 | Coxswain's POV over the ramp: beach, bluffs and smoke getting closer | Forward push, bumpy | "Point of view from the stern of a WWII landing craft over the steel ramp toward smoky Normandy beach and bluffs, rough sea, overcast, photorealistic" |

**Audio:** boat engine drone; hull slaps; muffled distant shellfire; drums enter.
**Transition:** match-cut on the ramp.

### SCENE 3 — OMAHA BEACH (15.0–25.0 s)
**Narration:** "At Omaha Beach, American soldiers faced devastating German resistance. Many never made it beyond the shoreline."
**Text:** **OMAHA BEACH**, stamped with location subtext "NORMANDY, FRANCE".

| Shot | Time | Visual | Camera | AI prompt (abridged) |
|---|---|---|---|---|
| 3A | 15.0–18.0 | Ramp drops; soldiers surge out into waist-deep surf | Handheld from inside the boat, the ramp falling toward the lens | "WWII landing craft ramp drops, American soldiers in 1944 uniforms rush into waist-deep surf at Omaha Beach, smoke, water splashes from gunfire, handheld chaotic camera, no gore, photorealistic" |
| 3B | 18.0–21.5 | Half-submerged view: soldiers wading between Czech hedgehogs, tracer splashes in the water | Camera at the waterline, partly underwater | "Waterline camera, American WWII infantry wading past steel Czech hedgehog obstacles, splashes, smoke over the beach, grey overcast, cinematic, no blood" |
| 3C | 21.5–25.0 | Soldiers crouching behind obstacles and a seawall; empty helmet rolling in the surf (loss implied, not shown) | Slow push, desaturated further | "American WWII soldiers sheltering behind beach obstacles and a shingle bank at Omaha Beach, smoke drifting, an empty M1 helmet rolling in the surf, sombre, no gore" |

**Audio:** MG-42 bursts (distant, filtered); near-miss whizzes; muffled underwater audio on 3B; music drops to a low pulse on "Many never made it…".
**Transition:** smoke wipe.

### SCENE 4 — THE BATTLE (25.0–35.0 s)
**Narration:** "Despite enormous losses, the Allied forces continued fighting, pushing through German defenses."
**Text:** **COURAGE UNDER FIRE**

| Shot | Time | Visual | Camera | AI prompt (abridged) |
|---|---|---|---|---|
| 4A | 25.0–28.5 | Infantry advance in short rushes between obstacles toward the bluffs | Handheld tracking alongside | "Allied WWII infantry advancing in short rushes across a smoky Normandy beach between wooden stakes and Belgian gates toward bluffs, 1944 uniforms, handheld tracking, photorealistic" |
| 4B | 28.5–31.5 | M4 Sherman emerges from smoke, tracks churning sand | Low angle, slow dolly back | "Low angle, WWII M4 Sherman tank emerging from thick smoke on a Normandy beach, tracks churning wet sand, infantry following behind, overcast, cinematic" |
| 4C | 31.5–35.0 | Medic with Red Cross brassard dragging a wounded soldier to cover (no visible wounds) | Medium, shallow DOF | "American WWII combat medic with Red Cross armband and helmet markings pulling a wounded comrade behind cover on the beach, no visible injuries, smoke, emotional, photorealistic" |

**Audio:** Sherman engine and track clank; orchestral build with heavy percussion.
**Transition:** a dissolve to calmer light carries the story off the beach.

### SCENE 5 — THE TURNING POINT (35.0–46.0 s)
**Narration:** "By the end of D-Day, the Allies had established a foothold in France. The liberation of Western Europe had begun."
**Text:** **THE LIBERATION HAD BEGUN**

| Shot | Time | Visual | Camera | AI prompt (abridged) |
|---|---|---|---|---|
| 5A | 35.0–40.0 | Troops crest the bluff; behind them the beach is crowded with landing craft and vehicles | Crane up from behind the soldiers to reveal the beachhead | "Crane shot rising behind Allied WWII soldiers reaching the top of a grassy bluff, revealing a Normandy beach below crowded with landing craft, vehicles and barrage balloons, late afternoon light breaking through clouds, epic, photorealistic" |
| 5B | 40.0–46.0 | British/Canadian section moving through a stone Normandy village street, past damaged houses, stained-glass church spire behind | Steady tracking shot backward ahead of the soldiers | "Tracking shot, British and Canadian WWII soldiers in khaki battledress and Mk III helmets advancing cautiously through a narrow stone Normandy village street, damaged houses, church spire, June 1944, golden late light, photorealistic" |

**Audio:** music turns heroic (brass); combat ambience recedes; birdsong in the village.
**Transition:** slow dissolve to a white flash at frame 1379.

### SCENE 6 — THE EMOTIONAL ENDING (46.0–55.0 s)
**Narration:** "D-Day came at a terrible cost. But their sacrifice helped change the course of history. We will never forget."
**Text:** **LEST WE FORGET** (or "WE WILL NEVER FORGET", your call). Slow fade in over 20 frames; held to the end. End card for the last 0.8 s: title "D-DAY: THE MORNING THAT CHANGED HISTORY".

| Shot | Time | Visual | Camera | AI prompt (abridged) |
|---|---|---|---|---|
| 6A | 46.0–51.0 | Rows of white marble Latin crosses and Stars of David on a green lawn at sunset | Very slow lateral dolly, low | "Normandy American Cemetery at sunset, endless rows of white marble Latin crosses and Stars of David on green lawn, long shadows, warm golden light, slow lateral dolly, serene, photorealistic, no people" |
| 6B | 51.0–55.0 | Wide view: the cemetery on the bluff, the sea beyond, the sun low | Slow crane up and back | "Wide crane shot of the Normandy American Cemetery on the bluff above Omaha Beach at sunset, white headstones in rows, English Channel beyond, calm and reverent, photorealistic" |

**Audio:** solo trumpet or strings; sea and gentle wind; music resolves on the final word; 0.5 s tail.

---

## 6. Subtitles

- **Style:** word-by-word "pop" captions, 2–4 words per line, Montserrat ExtraBold or Anton at about 78 px, white with an 8 px black outline and soft shadow, centred at y ≈ 1350 px.
- **Highlights:** dates and numbers (**JUNE 6TH, 1944**, **156,000**, **D-DAY**, **OMAHA BEACH**) shown in gold (#E8B33A) at 110% scale.
- **Timing:** word timings come from the TTS engine's timestamps (or forced alignment). A plain `.srt` is also exported for YouTube's caption upload.
- **No overlap:** the scene headline text sits in the upper third, subtitles in the lower-middle third.

## 7. Audio mix targets

- **Loudness:** narration-forward, integrated about **−14 LUFS**, true peak ≤ **−1 dBTP**.
- **Ducking:** music sidechain-ducked about 10 dB under the voice.
- **SFX:** sit below the voice and never mask a word.
- **Narrator:** deep, measured male voice, slight room reverb, light compression.

## 8. Export (FFmpeg, runs in this container)

```bash
ffmpeg -i master.mov -c:v libx264 -profile:v high -pix_fmt yuv420p -r 30 -s 1080x1920 \
  -crf 18 -preset slow -movflags +faststart -c:a aac -b:a 192k -ar 48000 -t 55 \
  d-day_short_final.mp4
```

**Verification before delivery:**
- ffprobe confirms resolution, frame rate, codec and duration ≤ 55.0 s.
- Frame grabs at each scene midpoint are checked for anachronisms and graphic content.
- Loudness is measured with `ebur128`.

## 9. Decisions needed from you

1. **Visual route:**
   - A, paid AI video: name the provider (Kling, Veo or Runway) and supply a key. I'll quote the cost first.
   - B, your own GPU.
   - C, free archival footage.
   - D, hybrid.
2. **Network:** allow the hosts your route needs in the environment's network settings:
   - Route A: the provider's API host.
   - Route C: `archive.org`, `commons.wikimedia.org`, `upload.wikimedia.org`.
   - Free Piper narration: `huggingface.co`.
3. **Voice:**
   - Piper (free).
   - ElevenLabs or Google (paid).
   - Your own recording.
4. **End text:** "LEST WE FORGET" (as briefed) or "WE WILL NEVER FORGET".

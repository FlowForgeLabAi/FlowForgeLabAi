Portfolio — draft (2026-10-09)
> English version of 作品集-初稿.md. Same rules: only claims that can be clicked and checked,
> no adjectives. The reader is an engineer or tech lead, not HR.
---
In one line
Running MiniMax H3 on an 8 GB laptop GPU — making it work, making it stable, and publishing
every number measured along the way.
---
What I have built
1. A working 8 GB laptop configuration for H3, published in full
`https://github.com/FlowForgeLabAi/MiniMax-H3-8GB-Workflows` (14 stars)
Three tiers: 20-step, 8-step, 4-step. A 10 s 768×1024 clip takes 14.3 min at 8 steps and
33.8–43.5 min at 20 steps.
2. Each trap on that path written up as a reproducible case
`https://github.com/FlowForgeLabAi/-h3-8gb-traps` (2 stars)
Six measured traps, each with its reproduction. The central finding: what stops you is not the
8 GB of VRAM, it is the 16 GB of system RAM.
3. Raw telemetry, published
`https://huggingface.co/datasets/FlowForgeLabAi/minimax-h3-8gb-bench` (37 downloads)
18 timed runs plus 534 samples of GPU monitoring data.
4. A self-check page, indexed on the H3 model page
`https://huggingface.co/spaces/FlowForgeLabAi/minimax-h3-8gb-advisor`
The six traps, a power-draw self-test, and a wall-clock predictor, `T ≈ 102 + 34.1 × steps`.
5. A ComfyUI bug report that an official fix PR cites
`https://github.com/Comfy-Org/ComfyUI/issues/16544`
`SaveVideo` fails to open libx264 when width or height is odd. The fix PR `#16545` carries
`Fixes #16544` in its description.
6. A technical exchange in the official H3 repository
`https://huggingface.co/MiniMaxAI/MiniMax-H3/discussions/104`
On the wall that a "stream from disk to run H3 under 4 GB of VRAM" approach runs into, and how
to tell whether you have hit it.
---
What I measured
Finding	Number
The bottleneck is system memory, not VRAM	Working set ≈ 34.5 GiB (19.5 GiB model + 15.0 GiB text encoder) against 15.26 GiB of RAM
GPU utilisation lies when it stalls	Healthy: 64–98 W. Stalled: flat 34–36 W while utilisation still reads 99%
Paging degrades throughput non-linearly	Same graph, same resolution: 0.9–6 s/block fresh, 212 s/block after ~1.7 h
Attention backends differ enormously at H3's real geometry	SDPA 143.1 ms / comfy-kitchen INT8 22.8 ms / SageAttention 29.0 ms
A LoRA applies to only part of a pruned model	208 of 259 modules (80.3%); all 51 `adaln\_proj.linear` skipped
The pinned-memory budget on Windows	`MAX\_PINNED\_MEMORY = ram \* 0.40` (≈6.1 GiB non-pageable)
---
I corrected my own error in public
After publishing, I went back to the source and found I had quoted the wrong constant: I had
written `ram \* 0.90`, which is the Linux branch. Windows takes `ram \* 0.40`.
Before writing the correction I first confirmed which branch I was actually on — because the
wrong branch gives 11.26 GiB and the right value is 6.10 GiB.
I corrected three published artefacts (the HF Space, a GitHub README, an in-flight PR) and kept
the one copy that had the platform right all along.
This section stays in on purpose. It is evidence of how I work, not a mark against it.
---
Currently working on
The usable envelope for H3 on 8 GB: resolution, step count, reference images
Turning the measurements above into a one-page self-check other people can run
---
Still to add
The full write-up behind the correction section is now public:
`https://github.com/FlowForgeLabAi/-h3-8gb-traps/blob/main/verification-and-correction-2026-10-01.md`
No education or employment section. Whether to include one, and how, is your call.

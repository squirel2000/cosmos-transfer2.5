# VLA Pipeline — Cosmos-Transfer 2.5

Concise guide for converting Isaac Lab simulation videos (Unitree G1) into photorealistic clips for Visual‑Language‑Action (VLA) training.

## Layout
- `vla_pipeline/configs/` — JSON configs (example: `sim2real_vla.json`).
- `vla_pipeline/data/input_video/` — place input mp4s here.
- `vla_pipeline/data/output/` — generated outputs.

## Setup
Follow `docs/setup.md` for environment and GPU setup (H100 recommended).

## Quick Start
1. Place your source video in /vla_pipeline/data/input_video/

```bash
vla_pipeline/data/input_video/episode_000001.mp4
```

- Do not arbitrarily resize videos — keep geometric consistency.
- Input dimensions must be divisible by 16 (e.g., 640x480 or 1280x720).

2. Edit `vla_pipeline/configs/sim2real_vla.json` if needed.

Notes: Use `control_weight` ~0.90–1.0 to preserve robot geometry and action labels. If you go lower (e.g., 0.7), the robot arm might look "real," but it might drift by 2-3cm. In robot manipulation, a 3cm error causes the grasp to fail.

3. Run inference (choose GPU):

```bash
export CUDA_VISIBLE_DEVICES=1 # Set "1" for SW2/Motion's application on Robot BU's H100
python examples/inference.py -i vla_pipeline/configs/sim2real_vla.json -o vla_pipeline/data/output
```

## Prompt Engineering (VLA-focused)
- Prioritize consistency over cinematic style.
- Be explicit about object locations and robot colors (repeat if necessary).
- Avoid "cinematic" or heavy shadows — prefer "flat" or "laboratory" lighting.
- Use materials ("glossy plastic", "rubber") to reduce hallucination.

Example Prompt:
- "Overhead camera footage of a white Unitree G1 humanoid robot with black joints. The robot has a grey plastic body and grey mechanical hands. It is interacting with a cabinet drawer. A ceramic mug is stored inside the drawer. On top of the cabinet is a surface with a clean empty rubber mat and a plastic bottle. Bright fluorescent office lighting, domestic kitchen, neutral colors, 4k, sharp focus."


### Commit to GitHub with Your Account
```bash
git add .
git -c user.name="TingYing Wu" -c user.email="tingying.wu@gmail.com" commit -m "Your commit message"
```
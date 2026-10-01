# CartPole report

Replace each **TODO** with your own observations. Keep this report short; link code,
compact results, and images instead of pasting logs.
For each stage, explain which tool or function uses your inputs, where the
results come from, and what uses them next. Point to the relevant code or
documentation; a few sentences or an annotated arrow diagram is enough.

To link a file in Markdown, use `[link text](relative/path/to/file)`.
For example, `[Environment code](../onboarding/env.py)` becomes
[Environment code](../onboarding/env.py). Links are relative to this report's
`docs/` folder; use `../` to reach the repository root.
To display an image, add `!`: `![Model screenshot](../results/model.png)`.

## Setup

- Member: **Oliver Ravin**
- OS: **MacOS**
- Setup diagnostic, viewer, and RGB results; any fix needed: **Small struggle getting MuJoCo to work with mjpython but working now**
- Fork URL and working branch: **https://github.com/oravin8/BSRA-RL**
- Before starting, read the [toolchain overview](../resources/toolchain.md).
  What role does each of MuJoCo, Gymnasium, Stable-Baselines3, and TensorBoard
  play in this exercise? Describe how they work together in your own words: 
  **MuJoCo is the place where the reinforcement learning can apply to a physical being through its joints and sensors. However behind it, gymnasium is used for as the backbone for creating reinforcement learning environments while Stable-Baslines3 saves the implementation used. TensorBoard also shows the progress of the reinforcement learning over time.**

## Model and task

### Model (Stage 2)

- Which tool loads `scene.xml` and its included `cartpole.xml`, and what does it
  create from them? Point to the loading call in `scripts/view_model.py`. Which
  tool computes the motion you see in the viewer when a control is applied?
  **The model was loaded in line 18 from scripts/view_model.py. MuJoCo then computes the motion through its simulation model when the model was loaded.**
- Which XML file owns the mechanism, and how does the include connect it to the
  scene? Explain the slide/hinge axes, unactuated pole, box half-extents, and
  degrees versus radians. Link a small model screenshot (`../results/model.png`):
  **The xml file is cartpole as that file contains the creation of the pole and the cart which is then connected through linking the file with the scene to get the full picture. In cartpole, the cart was created with a slider joint in the x-axis, then a pole with a y-axis hinge joint but does not contain an actuator since it does not have an applied force pushed on it. Both also have their own sizes in the shape of boxes. The XML specifically asked for degrees compared to radians which is why it contained -90 to 90 instead of -pi/2 to pi/2.**

### Environment (Stage 3)

Answer each row in a few sentences, using the source linked in
[Stage 3](ONBOARDING.md#3-environment-setup-and-inspection). Name the function or
setting that supports your answer, and distinguish MuJoCo's role from Gymnasium's.

| Topic        | Question                                                                                                                                                | Your answer |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| Observations | What are the four values returned to the policy, in order and with units? Where do those values come from, and how does `_get_obs()` assemble them?     | **The values are cart position (m), pole vertical angle (rad), cart linear velocity (m/s), and pole angular velocity (rad/s). The values come from the specific xml files where they are initialized and uses an observation space with the format: Box(-Inf, Inf, (4,), float64).**    |
| Actions      | What does the action control, what is its allowed range, and how does it reach the motor?                                                               | **In the xml, the action controls where the cart can move as its left-most limit and right-most limit. The current range is -3 to 3. It reaches the motor by the slider and the force that is applied on the cart.**    |
| Physics      | Which tool computes motion from your XML model, and how does Gymnasium ask it to advance? How much simulated time passes per action?                    | **Scene.xml contains an option to determine the gravity and the time step (0.02s) each action is done to simulate the world environment. However there is also a frame-skip option that does every other frame, so 0.04 seconds pass per action.**    |
| Reset        | What does `reset_model()` change at the start of an episode? How are the starting values randomized, and what does using the same reset seed reproduce? | **It changes the starting location on the grid  and they are randomized through a randomizer function. Using the same reset seed lets the agent start in the specific location the seed is associated with.**    |
| Reward       | What is the exact reward rule, which code computes it, and does it use the state before or after the action?                                            | **The reward rule is that if the angle is less than 0.2, then they get a +1 reward point which is computed through the v5 pendelum code. It uses the state after the action**    |

- Environment test and random-rollout results; action-range warning: **Warned for not using -1 to 1 as range as rather -3 to 3 was utilized.**

## Training

- Run name / source commit / training seed: **TODO**
- Requested and actual steps / elapsed time / device: **TODO**
- Configuration and versions: **TODO** link `../results/<run>/run.json`
- Learning curve: **TODO** embed `../results/<run>/learning-curve.png`

- What do SB3, PyTorch, and `Monitor` each do here? How does SB3 use the environment
  and settings you pass to PPO? **TODO**
- What do the curve's axes, averaging, and smoothing mean, and what evidence of
  improvement, variability, or a plateau do you see? **TODO**

## Evaluation

Use reset seeds 10000–10019, deterministic PPO actions, and random action seeds
`reset seed + 20000`. Standard deviations describe episodes (`ddof=0`).

| Agent | Episodes | Return mean ± std | Length mean ± std | Time-limit fraction |
| --- | --- | --- | --- | --- |
| Random | TODO | TODO | TODO | TODO |
| PPO | TODO | TODO | TODO | TODO |

- Per-episode CSV and summary JSON: **TODO** links
- Predetermined PPO rollout (seed 10000): **TODO** briefly describe the behavior you observed
- Limitation or failure, and a specific diagnosis if learning was weak: **TODO**

- Why does this evaluation differ from a training curve, and what can one
  trained seed not establish? What carries over to humanoid soccer, and what is
  missing (for example, contacts, partial observations, or sim-to-real transfer)? **TODO**

## Review

**TODO:** Record exact setup, test, train, and evaluate commands, plus any rendering
environment variable.

Keep the saved policy, TensorBoard logs, and rollout video locally and share them
directly with the RL lead if requested. No file uploads, download links, or GitHub
release are required for these files.

- Automated test results: **TODO**
- Manual model/render/video checks: **TODO**
- Fork PR link and review notes (record merge in the PR): **TODO**

## Feedback

- Did you learn anything from this onboarding? What was new, or what became
  clearer? If little was new, say so: **TODO**
- What improvements would make the onboarding easier to follow or more useful?
  Mention any confusing instructions, missing background, or unnecessary work: **TODO**

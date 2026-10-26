# Inverted Pendulum – Virtual Lab

A simulation of a robot that keeps a stick balanced upside down, made for **grade 10** students. It runs straight in the browser: nothing to install and no hardware needed.

👉 **Open the simulation:** `https://<your-username>.github.io/con-lac-nguoc/`
*(replace `<your-username>` with your GitHub username after turning on GitHub Pages)*

---

## How to use it

- **On a computer or phone:** open the link above, or scan the QR code your teacher shows you.
- **Without internet:** download `index.html` and double-click it to open it in Chrome, Edge, Firefox or Safari.

Each student can work on their own device. Finished challenges are remembered in that device's browser.

## The lesson has 3 steps

| Step | What students do | What they learn |
|---|---|---|
| **1. Balance it yourself** | Use the ← → keys or the on-screen buttons to keep the stick up | Unstable balance is very hard to hold. People react too slowly, so a computer is needed |
| **2. Teach the robot with PID** | Type in three numbers P, I and D, press **Apply**, then press **Push** to test | Each number has its own job. The robot stays balanced thanks to a feedback loop |
| **3. Swing up** | Watch the robot (or do it yourself) push back and forth to bring the stick from hanging to standing | Pushing in rhythm raises the mechanical energy step by step, like pumping a swing |

### What are P, I and D?

- **P – leans a lot → push hard.** Works like a spring that pulls the stick back to the middle.
- **I – leans for long → push more.** Adds up the lean over time. On this pendulum a big I actually makes the arm wander away, so I is usually left at 0.
- **D – falls fast → brake.** Looks at how fast the stick is falling and stops it early, so it does not wobble back and forth.

A good starting set: **P = 150, I = 0, D = 20**. There are 4 ready-made sets to compare (*Good set, No D, Small P, Big I*). Before you press Apply, the page **predicts** whether the new numbers will hold the stick, so students can check their own guesses.

### Is it balanced? (Lyapunov check)

Instead of judging "it looks upright" by eye, the page uses **Lyapunov's method**. It gives the robot a score **V**, a kind of "energy of the mistake":

- **V = 0** only when the stick is perfectly upright and everything is still.
- **100%** is the score of a stick leaning 5° and standing still.
- If V keeps going **down**, the robot is winning. Staying **below 10% for 1 second** counts as **Balanced ✓**.
- For numbers that cannot work, **no such V exists**, and the page says so before the stick even falls.

The verdict can be *Balanced ✓*, *Getting back to balance*, *Pushed off balance*, *Almost balanced (small shake)*, *Stick upright, arm still drifting*, *Can't balance* or *Fell over*.

<details>
<summary>How V is computed (for teachers)</summary>

Around the upright position the closed loop is linear: **ẋ = A·x** with x = [φ, φ', θ, θ'] (stick angle, its rate, arm angle, its rate). The page solves the Lyapunov equation **Aᵀ·X + X·A = −I** for the current P, I, D and uses **V(x) = xᵀ·X·x**. By Lyapunov's theorem for linear systems, a positive-definite X exists exactly when the loop is stable, and then V decreases along every motion.

With I > 0 the quantity h = φ' + c·θ' + b·φ − a·∫φ never changes, so ∫φ is folded into the other states and the arm settles at a new spot θ* = I·h / (a·k3). V is measured from that spot, which is why the page can say "stick upright, arm still drifting". The 10% threshold leaves room for the encoder's 0.36° steps, which make very large P or D shake slightly.
</details>

### 7 challenges

1. Keep the stick up yourself for 3 seconds.
2. Find numbers that let the robot balance for 10 seconds.
3. Push the stick 3 times and the robot still holds it.
4. Try D = 0 and see what the stick does.
5. Try I = 400, then press Push: does the arm wander far or stay close?
6. Watch the robot swing the stick up to the top.
7. Swing the stick up to the top yourself.

## Notes for teachers

The lesson fits into **4 periods**:

1. **Why is it hard?** Have the class balance a ruler or a broom on their hands, then do Step 1. Links to *types of equilibrium* and *moment of a force*.
2. **The feedback loop.** Go through Look → Think → Push at the bottom of the page. Game idea: one student balances a broom blindfolded while a partner calls out directions, to show how delay ruins balance.
3. **Tuning PID.** Students do Step 2 in groups: P only, then add D, then try I. They record results on a worksheet, and groups compete to finish the most challenges.
4. **Energy.** Do Step 3. Links to *potential energy, kinetic energy and conservation of mechanical energy*.

Quick check questions (no formulas needed):
- Why does the stick keep wobbling when only P is used?
- Draw the 3-step feedback loop.
- Why does the robot need to push back and forth many times to bring the stick to the top?

## What is the simulation based on?

It is based on the **[Arduino UNO Q Inverted Pendulum (PID UI)](https://projecthub.arduino.cc/Arduino_Genuino/arduino-uno-q-inverted-pendulum-pid-ui-e8833f)** project. The real build uses a NEMA 17 stepper motor to turn a horizontal arm and a 1000-step encoder to measure the stick's angle. This kind of pendulum is called a **Furuta pendulum** (rotary inverted pendulum).

In the simulation:
- The stick is a uniform rod 30 cm long on an arm 15 cm long. These sizes are estimates, because the original project does not publish them.
- The robot measures the angle and works out its push **500 times per second**. Like the real sensor, the encoder only notices changes of 0.36° or more.
- The motor has a strength limit, so making P bigger and bigger does not make the robot push harder forever.
- The controller students use is a simple PID plus a small "helper" that stops the arm from drifting away. It is easier to understand than the PID in the original Arduino firmware.
- In Step 3 the robot pumps energy in time with the swing. When the stick gets near the top, the robot switches to a built-in balancing controller.

## Files

```
index.html   ← the whole simulation (HTML + CSS + JavaScript in one file)
README.md    ← this file
```

To change anything, open `index.html` in any text editor, edit it, and upload it again.

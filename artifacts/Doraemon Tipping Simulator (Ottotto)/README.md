# Doraemon Tipping Simulator ("Ottotto")

An interactive 3D simulator that shows how far a Doraemon-like character tips over when you change the weight of the object held in his raised hand. It is a hobby project that combines a simple statics/dynamics model with a three.js scene.

> Unofficial fan-made project. Not affiliated with or endorsed by the rights holders of Doraemon.

**[Live demo](https://chi1180.github.io/ArtifactsByClaude/artifacts/Doraemon%20Tipping%20Simulator%20(Ottotto)/Doraemon%20Tipping%20Simulator%20(Ottotto).html)**

## Features

- Real-time 3D scene (three.js), drag to orbit the camera
- Two sliders: mass of the held object (0–250 kg) and arm angle φ (0–80° above horizontal)
- The held dorayaki grows with its mass
- Live readouts: critical mass `m_c`, ratio `m / m_c`, tilt angle
- Gauge with a center line at the tipping limit, plus a status badge: *Stable*, *Ottotto… (wobbling)*, *Tipping!*, *Fell over!*
- Reset button to stand him back up

## How to use

1. Move **Mass of held object** to change the weight in his hand.
2. Move **Arm angle φ** to change how far the hand reaches outward.
3. Watch the gauge. Once `m / m_c` reaches 1 (the center line), he starts to tip over.
4. Press **Reset** to stand him up again.

## Physics model

The model is planar (2D, side-on) and treats the body as a rigid body that can rotate about the **outer edge of the foot on the arm side** (the red marker in the scene). The shoulder is assumed to be directly above this pivot.

### Static tipping condition

Torque balance about the pivot at the moment of tipping:

$$
Mg\,\frac{w}{2} = mg\,l\cos\varphi
\quad\Longrightarrow\quad
m_c = \frac{wM}{2\,l\cos\varphi}
$$

| Symbol | Meaning | Value in the simulator |
|---|---|---|
| `M` | Body mass | 129.3 kg |
| `w` | Width between the outer edges of the feet | 0.6 m |
| `l` | Arm length | 0.35 m |
| `φ` | Arm angle above horizontal | slider, 0–80° |
| `m` | Mass of the held object | slider, 0–250 kg |
| `h_c` | Height of the body's center of mass | 0.55 m |
| shoulder height | Height of the shoulder pivot | 0.62 m |

If `m < m_c` the body stays upright; if `m > m_c` gravity torque from the hand wins and he tips outward.

### Dynamics

The tilt angle α (positive = outward) follows

$$
I\,\ddot\alpha = \tau_g(\alpha) + \tau_{\text{sole}}(\alpha) - c\,\dot\alpha
$$

with

$$
\tau_g(\alpha) = g\Big[M\big(-\tfrac{w}{2}\cos\alpha + h_c\sin\alpha\big) + m\big(d\cos\alpha + h_h\sin\alpha\big)\Big]
$$

where `d = l cos φ` is the horizontal reach of the hand, `h_h = 0.62 + l sin φ` is the hand height, and `I` is the moment of inertia about the pivot (body treated with an approximate radius of gyration, hand as a point mass).

- **Sole compliance:** a soft spring acts for small outward tilt (`k_out = 1.15·M·g·h_c`, vanishing at α_s = 0.3 rad, i.e. when the foot lifts), and a stiff spring acts on the inward side (`k_in = 30·M·g·h_c`) to represent the other foot on the ground.
- **Damping:** `c = 30`.
- **Integration:** semi-implicit Euler, 10 sub-steps per frame. The fall is stopped at 1.45 rad (about 83°) with a small bounce.

## Limitations

- The arm's own mass is ignored; only the held object (a point mass at the hand) is modeled.
- The shoulder is placed exactly above the pivot, and the body is a single rigid piece.
- No stepping or balance recovery. Real "ottotto" involves taking a step, which is not simulated.
- The wobble and the swinging free arm shown as the ratio approaches 1 are a **visual cue only**; the physics-driven motion is the tipping that happens beyond the limit.
- Parameters are rough estimates chosen for a readable demo, not measured values.

## Running locally

The whole app is a single HTML file: `Doraemon Tipping Simulator (Ottotto).html`.

1. Download the file.
2. Open it in a modern browser. An internet connection is needed to load three.js from the cdnjs CDN.

To work offline, download `three.min.js` (r128) and change the `<script src=...>` tag to point to the local copy.

## Customizing

All model parameters are constants at the top of the `<script>` block:

```js
const g=9.8, M=129.3, w=0.6, l=0.35, hc=0.55, sy=0.62;
const kout=1.15*M*g*hc, kin=30*M*g*hc, as=0.3;
```

Changing `M`, `w`, `l` or `hc` changes the tipping threshold. The slider ranges are set in the `<input type="range">` elements.

## Tech

- [three.js](https://threejs.org/) r128 (UMD build from cdnjs)
- Plain HTML, CSS and JavaScript, no build step
- Light and dark theme through `prefers-color-scheme`

## License

_Choose a license for your code (for example MIT) and add it here._

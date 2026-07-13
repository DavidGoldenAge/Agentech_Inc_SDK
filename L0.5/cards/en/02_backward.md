# `Agentech.backward(parameters)`

<!-- START: Definition -->
## Definition

**L0.5 · Movement** — Send a bounded nonpositive body-frame `vx` command, then send the zero-motion command.

This is an open-loop wrapper around ZSL-1 `move(vx, vy, yaw_rate)`. It does not claim measured distance or odometry completion.
<!-- END: Definition -->

<!-- START: Syntax -->
## Syntax

```python
Agentech.backward()
Agentech.backward(speed_mps: float, duration_s: float)
Agentech.backward(speed_percent: int, duration_s: float)
Agentech.backward(speed_level: int, duration_s: float)
Agentech.backward(pace: str, duration_s: float)
Agentech.backward(distance_m: float, speed_mps: float)
```
<!-- END: Syntax -->

<!-- START: Constraints -->
## Constraints

1. Select exactly one speed representation: `speed_mps`, `speed_percent`, `speed_level`, or `pace`. `distance_m` may be paired only with `speed_mps`.
2. Mixed speed representations return `rejected(E_PROFILE_MIXED)`.
3. Values outside the published range return `rejected(E_RANGE)`; the SDK does not clamp them.
4. A distance call is accepted only when `distance_m / speed_mps <= 10.0 s`.
5. ZSL-1 `move` may be entered only from standing state.
6. The same profile resolver must be used by robot and simulator backends.
7. `speed_level=0` resolves to `0.0 m/s`; integer levels `1..512` resolve monotonically to `0.05..3.0 m/s`.
<!-- END: Constraints -->

<!-- START: Defaults -->
## Defaults

| Call | Deterministic resolution |
| --- | --- |
| `Agentech.backward()` | `speed_mps=1.2`, `duration_s=1.0` |
| Any speed selector without `duration_s` | `duration_s=1.0` |
| `Agentech.backward(distance_m=...)` | `speed_mps=1.2`; duration is `distance_m / 1.2` |
<!-- END: Defaults -->

<!-- START: Parameters -->
## Parameters

| Profile | Range / values | Resolution to speed |
| --- | --- | --- |
| `speed_mps` | `0.05 <= speed_mps <= 3.0` | unchanged magnitude; backend receives negative `vx` |
| `speed_percent` | integer `2..100` | `speed_percent / 100 * 3.0 m/s` |
| `speed_level` | integer `0..512` | `0 -> 0.0 m/s`; otherwise `0.05 + (speed_level - 1) * 2.95 / 511 m/s` |
| `pace` | `"slow"`, `"normal"`, `"fast"` | `{slow:0.6, normal:1.2, fast:2.4} m/s` |
| `distance_m` | `0 < distance_m <= 3.0` | duration = `distance_m / speed_mps` |
| `duration_s` | `0 < duration_s <= 10.0` | command duration |

The ZSL-1 backend accepts `vx=0` or nonzero `|vx|` from `0.05` through `3.0 m/s`. Agentech exposes the full backward command-magnitude range `0.05..3.0 m/s`; the backend receives the resolved value as negative `vx`. `speed_percent`, `speed_level`, and `pace` are Agentech aliases with the exact mappings above; they are not upstream ZSL-1 parameters or measured speeds.

`speed_level` is an Agentech integer command level with 513 states. Level `0` means zero motion. Levels `1..512` are distributed linearly across the valid nonzero backend magnitude range `0.05..3.0 m/s`, so every nonzero level resolves to a legal ZSL-1 command and level `512` resolves to full speed.

### Reserved TBD parameters

| Previously designed field | Status |
| --- | --- |
| `step_count` | `TBD` — no ZSL-1 step-count command or conversion source found |
| `step_rate_hz` | `TBD` — no ZSL-1 step-rate parameter found |
| `gait` | `TBD` — no ZSL-1 gait selector found |

These fields remain reserved for compatibility planning. Passing any of them currently returns `rejected(E_TBD_PARAMETER)` before a robot or simulator command is emitted.
<!-- END: Parameters -->

<!-- START: Behavior -->
## Behavior

After the robot is motion-ready, the resolved command is `move(-speed_mps, 0, 0)`. At the resolved duration the SDK sends `move(0, 0, 0)`. If `speed_level=0`, the SDK sends only `move(0, 0, 0)`. A simulator must receive these same numeric commands and timing; it must not reinterpret a level or pace independently.
<!-- END: Behavior -->

<!-- START: Return -->
## Return

```python
SkillResult(status, trace_id, error_code, message)
```

`status` is `"succeeded"`, `"rejected"`, `"preempted"`, `"estopped"`, or `"timeout"`. Upstream return codes are translated through `profiles/aegis/zsl1.yaml`; the numeric upstream code remains in the command trace.
<!-- END: Return -->

<!-- START: Example -->
## Example

```python
result = Agentech.backward(speed_level=256, duration_s=1.0)  # move(-1.522, 0, 0)
result = Agentech.backward(pace="fast", duration_s=1.0)      # move(-2.4, 0, 0)
result = Agentech.backward(distance_m=0.3, speed_mps=0.25)   # 1.2 s
```
<!-- END: Example -->

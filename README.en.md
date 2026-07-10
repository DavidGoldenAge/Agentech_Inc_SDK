# Agentech SDK Cards

This is the English reading entry for the Agentech SDK card repository.

Use this page when you want the English API contract for Aegis robot-dog telemetry and bounded atomic skills. The card files stay inside the layer folders; this root-level README is only an index and reading guide.

## What This English Entry Covers

| Layer | Count | Scope |
| --- | ---: | --- |
| `L0.0` | 1 | Direct telemetry snapshot card |
| `L0.5` | 12 | Bounded atomic movement, posture, safety, and sensing/posture skills |

## L0.0 English Card

| API | Category | Card |
| --- | --- | --- |
| `Agentech.get_battery_status(parameters)` | Telemetry | `L0.0/cards/get_battery_status.en.md` |

## L0.5 English Cards

| # | API | Category | Card |
| ---: | --- | --- | --- |
| 01 | `Agentech.forward(parameters)` | Movement | `L0.5/cards/en/01_forward.md` |
| 02 | `Agentech.backward(parameters)` | Movement | `L0.5/cards/en/02_backward.md` |
| 03 | `Agentech.lateral(parameters)` | Movement | `L0.5/cards/en/03_lateral.md` |
| 04 | `Agentech.turn(parameters)` | Movement | `L0.5/cards/en/04_turn.md` |
| 05 | `Agentech.twist(parameters)` | Movement | `L0.5/cards/en/05_twist.md` |
| 06 | `Agentech.backflip(parameters)` | Movement | `L0.5/cards/en/06_backflip.md` |
| 07 | `Agentech.jump(parameters)` | Movement | `L0.5/cards/en/07_jump.md` |
| 08 | `Agentech.stand(parameters)` | Posture | `L0.5/cards/en/08_stand.md` |
| 09 | `Agentech.sit(parameters)` | Posture | `L0.5/cards/en/09_sit.md` |
| 10 | `Agentech.stop(parameters)` | Safety | `L0.5/cards/en/10_stop.md` |
| 11 | `Agentech.emergency_stop(parameters)` | Safety | `L0.5/cards/en/11_emergency_stop.md` |
| 12 | `Agentech.look(parameters)` | Sensing/Posture | `L0.5/cards/en/12_look.md` |

## How To Read A Card

Read each card in this order:

1. `Definition`: confirm the layer, category, and purpose.
2. `Syntax`: copy the valid callable shape.
3. `Constraints`: check selector rules and rejection behavior.
4. `Defaults`: confirm what omitted parameters do.
5. `Parameters`: check ranges, units, mappings, and engineering notes.
6. `Behavior`: understand runtime execution.
7. `Return`: branch on stable return fields.
8. `Example`: use concrete values from real calls.

## English Style Contract

English cards use concise engineering language. The text should make the public API clear without exposing internal controller details, device-specific implementation names, or undocumented recovery logic.

All Python SDK parameters use `snake_case`, and `Syntax` shows parameter names and types only. Concrete values belong in `Example`.

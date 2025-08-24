# Sensor Fusion Toolkit (SFTK)

**Deterministic C++20 sensor-fusion for industrial autonomy.**  
Fuses GNSS, IMU, wheel odom, LiDAR odom, visual odom, magnetometer, barometer into one reliable state.  
Targets ROS2, industrial PCs, and PLC/embedded bridges via a C ABI.

> **Status:** Repo scaffold. Code landing after ICTK base (Aug 31). This README defines scope and gates.

---

## Why SFTK
Typical stacks glue ROS nodes, vendor SDKs, and paper code. They fail under noise, clock drift, dropouts, and out-of-order data. SFTK is built for field conditions and hard real-time budgets.

**Differentiators**
- **Deterministic runtime:** fixed-step, no heap after init.
- **Time discipline:** clocking, sync, buffering, and replayable logs.
- **OOO tolerant:** bounded reordering windows and consistency checks.
- **SafeMode:** health scoring, graceful degradation, recovery.
- **Unified API:** swap estimators without re-wiring integration.
- **Portable:** ROS2 optional; C ABI for PLC/embedded bridges.

---

## Scope (planned)

**Estimators**
- *Kalman family:* ESEKF (first), EKF, IEKF, UKF, SR-EKF  
- *Information filters:* IF, SRIF  
- *Robust/adaptive:* H∞, adaptive covariance tuning  
- *Sampling:* PF, RBPF  
- *Optimization:* MHE, batch smoother

**Motion models**
- CV, CTRV, holonomic, SE(2)/SE(3) IMU-driven, Z-bounded ground vehicles

**Measurement models**
- IMU, GNSS (LLA/ENU), wheel odom, LiDAR odom, visual odom, magnetometer, barometer

**Features**
- Lifecycle: `init → predict → update` with strict timing contracts
- Health: per-sensor quality, gating, dropout handling, bias/misalignment modeling
- Calibration tools: bias, misalignment, scale, timestamp offset; field CLIs
- Serialization: snapshot/restore, bias cache; schema versioning and migrations
- Adapters: thin ROS2 nodes in `autonomy-adapters-ros2/sftk_fusion_bridge`
- Docs: pipelines, failure modes, KPI reference, operations playbook

---

## Performance targets & KPI gates
- **Throughput:** ≥1 kHz IMU + ≥20 Hz exteroceptive with headroom
- **Latency:** zero deadline misses in WCET harness at target tick rate
- **Consistency:** NEES/NIS within bounds on public benches
- **Drift/outage:** bounded drift during GNSS outages; documented recovery time
- **Determinism:** no runtime allocations after init; reproducible runs
- **Acceptance:** merges gated by KPI uplift vs baselines and passing consistency checks

---

## Roadmap
- **Aug–Sept:** Core infra + ESEKF pipeline + SafeMode + minimal ROS2 demo
- **Q4:** EKF/UKF/IEKF/SR-EKF + MHE baseline + calibration CLIs + dataset runners
- **Later:** PF/RBPF, smoother, embedded/PLC bridges, full docs and ops playbook

---

## Repository structure (planned)
```

sftk/
├─ include/sftk/
│  ├─ core/        # state, time, buffers, health
│  ├─ filters/     # ESEKF/EKF/UKF/IEKF/SR, IF/SRIF, PF, MHE, smoother
│  ├─ models/      # motion + measurement models
│  ├─ robust/      # H∞, adaptive covariances, SafeMode
│  ├─ sync/        # time sync, OOO windows, stamping
│  ├─ calib/       # bias/misalignment tools
│  ├─ io/          # logs, serialization
│  └─ c\_api/       # C ABI
├─ src/
├─ configs/fusion/ # pipeline YAMLs
├─ examples/       # ROS2 + standalone demos
├─ tests/          # unit, property, fuzz, determinism, WCET harness
├─ benchmarks/     # dataset runners, reports
├─ tools/          # calibration CLIs
└─ docs/           # guides, failure modes, KPI reference, ops playbook

```

---

## Integration
- **ROS2:** `autonomy-adapters-ros2/sftk_fusion_bridge`
- **PLC/embedded:** C ABI shims and examples (planned)
- **Upstream stack:** pairs with **ICTK** (control) and **RLTK** (RL runtime)

---

## License
MIT. 

---

## Links
- Control: [`industrial-control-toolkit`](https://github.com/1Kaustubh122/industrial-control-toolkit)
- RL runtime: [`reinforcement-learning-toolkit`](https://github.com/1Kaustubh122/reinforcement-learning-toolkit)
- ROS2 adapters: `autonomy-adapters-ros2` (incoming)

---

## Contributing
Focused roadmap. PRs welcome for determinism, sync/OOO handling, estimators, tests. 


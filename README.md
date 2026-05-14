# wsn-audio-smoothing-tcn
TCN-based pipeline for smoothing jittered UDP audio streams over wireless sensor networks.
### TCN-based Predictive Codec Controller

## Overview
A Temporal Convolutional Network (TCN) pipeline that proactively smooths 
jittered UDP audio streams over Wireless Sensor Networks — reducing 
end-to-end latency by 60% and packet loss by 60% compared to reactive baselines.

Based on the paper:
**"AI-Powered Low-Latency Communication via Predictive Codec Control"**
*Accepted at DSI-DMMES 2k26 (joint with KMUTNB, Thailand) — 
pending Scopus-indexed publication.*

## The Problem
Conventional real-time audio systems are **reactive** — they detect 
degradation after it occurs, causing perceptible interruptions. 
This system is **predictive** — it forecasts degradation and adapts 
codec settings before quality drops.

## Architecture
Network Telemetry → TCN Prediction Engine → Decision Controller → Codec Adapter → Audio Output
↑                                                                                        |
└────────────────────────────────── Feedback Loop ──────────────────────────────────────┘

## TCN Model
- 8 temporal convolutional layers
- Dilation factors: [1, 2, 4, 8, 16, 32, 64, 128]
- Receptive field: 256 samples (2.56 seconds of history)
- ~850,000 parameters
- Inference time: **3.2ms on CPU**
- Predicts latency, jitter, and packet loss at 100ms / 250ms / 500ms horizons

## Results
| Metric | Baseline (Reactive) | This System |
|---|---|---|
| End-to-End Latency | 300ms | 120ms (**↓60%**) |
| Packet Loss | 5% | 2% (**↓60%**) |
| MOS Score | 2.8 | 4.2 (**↑50%**) |
| PESQ | 2.1 | 3.8 |
| STOI | 0.68 | 0.89 |

## Tech Stack
- **Language:** Python
- **ML Framework:** PyTorch
- **Audio:** PyAudio, Opus codec (wideband/narrowband)
- **Networking:** Python Sockets, UDP
- **Network Emulation:** Clumsy, NetEm/tc
- **Monitoring:** Wireshark, InfluxDB, Grafana
- **API:** FastAPI

## How to Run
```bash
git clone https://github.com/mahika-ravi/wsn-audio-smoothing-tcn.git
cd wsn-audio-smoothing-tcn
pip install -r requirements.txt
python main.py
```

## Publication
Paper accepted at **DSI-DMMES 2k26** 
(jointly with King Mongkut's University of Technology North Bangkok, Thailand).
Pending Scopus-indexed publication.

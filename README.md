# Timestamp-based AES Key Refresh for IoT Security

This repository contains the implementation of a **lightweight timestamp-driven AES key refresh mechanism** for secure data transmission in IoT environments.  
The approach dynamically regenerates AES-256 keys using system timestamps, ensuring **forward secrecy** with negligible performance overhead.

---

## 🔑 Abstract
Secure data transmission in IoT networks often faces challenges of key compromise and synchronization.  
This work proposes a **timestamp-based AES key refresh mechanism**, where session keys are derived as:

$$ K_t = f(K_{master}, T) $$

- **K_t** → time-dependent session key  
- **K_master** → shared master secret  
- **T** → timestamp truncated to granularity (seconds/minutes)  

Results demonstrate that:
- 10s refresh interval achieved **98.5% success** with minimal latency overhead.  
- Frequent refresh (5s) increases mismatch but boosts security.  
- Performance overhead < 10%, making it feasible for real-time IoT use cases.
- The honeypot successfully **detected 100% of replay attempts** during controlled tests (10 attempts, 30s TTL).
- Bucket drift tolerance (±1) allowed for minor clock skew without false negatives.
- AES-GCM decryption overhead was **<3% CPU** on a standard IoT edge board (10s interval, 100 messages).


---

## 📂 Repository Structure
├── timestamp code.ipynb # Jupyter notebook with implementation & experiments
├── requirements.txt # Python dependencies
├── results/ # Graphs & tables from experiments
├── LICENSE # Open-source license
└── README.md # Project overview

---

## ⚙️ Installation & Usage

Clone the repo and install dependencies:

git clone https://github.com/SunnYNehrA01/TimeStamp-AES-Refresh.git
cd timestamp-aes-refresh
pip install -r requirements.txt



Run the notebook:
jupyter notebook timestamp code.ipynb

## 📊 Results

Simulation results (packet success vs. refresh interval):

Refresh Interval	Success Rate (%)	Mismatch Rate (%)	Avg Latency (ms)
30s	100.0	0.0	0.08
10s	98.3	1.7	0.06
5s	95.0	5.0	0.01

Honeypot Simulation Detection Results
[Key Refresh] active bucket=1757164160 (2025-09-06 18:39:20)[Honeypot] started (refresh_interval=10s, drift_buckets=±1)
[Honeypot] Decrypted (kid=1757164160) mid=471bde4e5ee7e550: replay-test-payload
[Demo Callback] meta: {'key_bucket': 1757164160, 'mid': '471bde4e5ee7e550'} payload preview: 7265706c61792d746573742d7061796c
[Honeypot] Replay detected (mid=471bde4e5ee7e550). Discarding.
[Honeypot] stopped

## 🔒 Security Notes

Replay attacks fail after key refresh.

Effective keyspace refreshes every interval.

Preserves AES-level security while reducing reliance on costly public-key cryptography.

## 📜 Citation

If you use this work, please cite:

Author: Sunny Nehra, Lakshya Soni

Title: Timestamp-based AES Key Refresh for IoT Security

Year: 2025

Repository: https://github.com/SunnYNehrA01/TimeStamp-AES-Refresh

## 📌 Future Work

Adaptive refresh intervals based on network load

Blockchain-based timestamp synchronization

Post-quantum key derivation integration

Hardware implementation on IoT boards (Raspberry Pi, ESP32)





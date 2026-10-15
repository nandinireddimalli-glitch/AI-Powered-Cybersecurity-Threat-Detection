# AI-Powered Cybersecurity Threat Detection System

A beginner-friendly, portfolio-oriented intrusion-detection prototype using Python, synthetic network-flow data, Random Forest classification, Isolation Forest anomaly detection, and SOC-style CSV alerts.

> **Safety and scope:** This project generates synthetic records locally. It does not capture network packets, attack live systems, or connect to corporate infrastructure. Results on synthetic data are not evidence of real-world detection performance.

## Features
- Creates a reproducible synthetic network-flow dataset when no dataset exists.
- Simulates benign traffic and three labeled patterns: DoS-like, brute-force-like, and port-scan-like.
- Cleans missing values and prepares nine numeric features.
- Trains a Random Forest binary classifier and an Isolation Forest anomaly detector.
- Reports accuracy, precision, recall, F1, confusion matrix, and classification report.
- Exports predictions, SOC-style alerts, JSON metrics, trained models, and four charts.

## Tech stack
Python 3.11, Pandas, NumPy, scikit-learn, Matplotlib, Seaborn, Joblib.

## Architecture
```text
Synthetic network-flow generator / CSV
                 |
                 v
        Data cleaning + features
                 |
          +------+------+
          |             |
          v             v
 Random Forest      Isolation Forest
 classifier         anomaly detector
          |             |
          +------+------+
                 v
     Prediction + alert severity
                 |
       +---------+----------+
       |         |          |
       v         v          v
   CSV alerts  JSON metrics  PNG charts
```

## Requirements
- Python 3.11 recommended (64-bit)
- VS Code with Python extension
- Internet access only for installing packages; execution is local
- No GPU required

## Windows setup (PowerShell)
Open this folder in VS Code, then select **Terminal → New Terminal**.

```powershell
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python main.py
```

If PowerShell blocks virtual-environment activation, use:
```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
```
This changes the execution policy for the current PowerShell process only. Alternatively, skip activation and run `.\.venv\Scripts\python.exe -m pip install -r requirements.txt` and `.\.venv\Scripts\python.exe main.py`.

## macOS / Linux
```bash
python3.11 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python main.py
```

## Run a single sample prediction
After running the pipeline once:
```bash
python -m src.predict
```

## Outputs
After `python main.py` completes:
- `data/synthetic_network_traffic.csv` — generated training/evaluation data
- `models/random_forest_classifier.joblib` — classifier
- `models/isolation_forest_detector.joblib` — anomaly detector
- `outputs/test_predictions.csv` — holdout predictions and anomaly flags
- `outputs/security_alerts.csv` — medium/high priority alerts
- `outputs/evaluation_metrics.json` — metrics and confusion matrix
- `images/traffic_distribution.png`
- `images/confusion_matrix.png`
- `images/anomaly_detection.png`
- `images/feature_importance.png`

The numeric metrics depend on the generated dataset and package versions. Because labels are generated from deliberately distinct synthetic patterns, metrics can be optimistic. Do not present them as performance on real networks.

## Dataset description
The built-in dataset is synthetic and generated locally with a fixed random seed. It contains flow-level numerical features: duration, source/destination bytes, packet count, failed-login count, connection rate, unique ports, protocol encoding, and hour of day. The `attack_type` column documents the simulated scenario and is not used as a model feature.

For a future extension, evaluate on a reputable public dataset such as UNSW-NB15 or CICIDS, checking its license, label definitions, feature compatibility, and train/test leakage before reporting results.

## Alert logic
- **HIGH:** classifier predicts attack with attack probability >= 0.80
- **MEDIUM:** classifier predicts attack, or Isolation Forest flags an anomaly
- **LOW:** neither condition is met (low alerts are retained in the prediction file)

This is a demonstration rule, not a calibrated production severity policy.

## GitHub screenshots
Add screenshots under `docs/screenshots/` and embed them like:
```markdown
![Confusion matrix](docs/screenshots/confusion_matrix.png)
![Feature importance](docs/screenshots/feature_importance.png)
```

## Suggested GitHub topics
`cybersecurity`, `threat-detection`, `intrusion-detection`, `machine-learning`, `python`, `scikit-learn`, `anomaly-detection`, `soc-analytics`

## Learning outcomes
- Data preprocessing and feature engineering
- Supervised binary classification and unsupervised anomaly detection
- Evaluation with precision, recall, F1, and confusion matrix
- Reproducible ML pipeline and model serialization
- Rule-based alert generation and security reporting
- Git/GitHub documentation and proof-of-work practices

## Limitations and next steps
1. Test on a public benchmark dataset with documented preprocessing.
2. Add time-based or source-host-based splits to reduce leakage.
3. Tune thresholds using a validation set and report false-positive rates.
4. Add a Streamlit dashboard only after the CLI pipeline is stable.
5. Never connect this demo to live systems without authorization and a proper security review.

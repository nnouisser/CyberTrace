<div align="center">

# 🛡️ CyberTrace

### AI-Powered Network Traffic Analysis & Anomaly Detection System

[![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python)](https://python.org)
[![Elasticsearch](https://img.shields.io/badge/Elasticsearch-9.3.2-005571?logo=elasticsearch)](https://elastic.co)
[![Kibana](https://img.shields.io/badge/Kibana-9.3.2-005571?logo=kibana)](https://elastic.co/kibana)
[![Zeek](https://img.shields.io/badge/Zeek-8.1.1-00A4EF)](https://zeek.org)

</div>

---

## 📋 Table of Contents

- [Overview](#overview)
- [Pipeline Architecture](#pipeline-architecture)
- [AI Model](#ai-model)
- [Tech Stack](#tech-stack)
- [Results](#results)
- [Dashboard Screenshots](#dashboard-screenshots)

---

## 🔍 Overview

CyberTrace is a complete automated network intrusion detection system that combines:

- Network traffic analysis (Zeek IDS)
- Machine learning (Random Forest)
- Data visualization (Kibana)
- Alert automation (N8N)

It detects anomalies with **97% accuracy**.

---

## 🏗️ Pipeline Architecture

![Pipeline Architecture](docs/architecture/pipeline_global.png)

### Data Flow

| Step | Component | Input | Output |
|------|-----------|-------|--------|
| 1 | Attack Simulation | Network traffic | Raw packets |
| 2 | PCAP Capture | Raw packets | `.pcap` |
| 3 | Zeek IDS | `.pcap` | JSON logs |
| 4 | Logstash | logs | Elasticsearch |
| 5 | Elasticsearch | data | Index |
| 6 | AI Model (Colab) | CSV | Predictions |
| 7 | Kibana | ES | Dashboard |
| 8 | N8N | anomalies | Email alerts |

---

## 🧠 AI Model

- **Algorithm**: Random Forest (scikit-learn)
- **Dataset**: NSL-KDD (125,973 samples)
- **Classification**: Binary (NORMAL / ANOMALY)

### Model Performance

![Model Results](docs/screenshots/colab_training.PNG)
![Model Results](docs/screenshots/mat.PNG)


### Severity Levels

| Level | AI Score | Action |
|------|----------|--------|
| 🔴 CRITICAL | ≥ 0.90 | Immediate action |
| 🟠 HIGH | 0.75–0.89 | Investigation |
| 🟡 MEDIUM | 0.60–0.74 | Monitoring |
| 🟢 LOW | 0.50–0.59 | Watch list |

---

## 📊 Results

- ✅ NORMAL: 1,528 (15.3%)
- 🚨 ANOMALY: 8,472 (84.7%)
- 🎯 Avg AI score: 0.939


------



## 🚀 Quick Start (Summary)

```bash
# 1. Start ELK Stack
C:\elk\elasticsearch-9.3.2\bin\elasticsearch.bat
C:\elk\logstash-9.3.2\bin\logstash.bat -f C:\elk\logstach.conf
C:\elk\kibana-9.3.2\bin\kibana.bat

# 2. Analyze PCAP with Zeek (WSL2)
mkdir -p /mnt/c/elk/zeek_output && cd /mnt/c/elk/zeek_output
zeek -r /mnt/c/elk/capture_totale.pcap LogAscii::use_json=T

# 3. Verify data
curl http://localhost:9200/zeek-*/_count

# 4. Run AI model (Google Colab)
# Upload cybertrace_colab.ipynb → run all cells → download resultats_ia.csv

# 5. Inject AI results
python inject_results.py

# 6. Import Kibana dashboard

curl -X POST "http://localhost:5601/api/saved_objects/_import?overwrite=true" \
  -H "kbn-xsrf: true" \
  --form file=@kibana/cybertrace_dashboard.ndjson
```
## 📸 Screenshots
###  Zeek Output
![Zeek Output](docs/screenshots/zeek.png)

### Kibana Discover

![Discover](docs/screenshots/Discover.PNG)

### Dashboard

![Dashboard 1](docs/screenshots/kibana_dashboard1.PNG)

![Dashboard 2](docs/screenshots/kibana_dashboard2.PNG)

![Dashboard 3](docs/screenshots/kibana_dashboard3.PNG)

![Dashboard 4](docs/screenshots/kibana_dashboard4.PNG)

### N8N Workflow


![N8N Workflow](docs/screenshots/n8n_workflow.PNG)

---

## 📄 License

This project is licensed under the MIT License — see LICENSE file.

---


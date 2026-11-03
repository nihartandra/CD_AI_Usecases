# Machine Learning for the SOC 🛡️

**Hands-on, beginner-friendly labs that teach security analysts how machine learning detects real threats, no data-science background needed.**

This repo holds two complete training sessions I built for my Cyber Defense team. Each one pairs a slide deck that explains the ideas in plain English with a Jupyter notebook that analysts run themselves. The examples use problems a SOC already deals with every day, and the explanations compare ML to work analysts already do: triage, detection rules and alert tuning.

---

## 👋 About me

I'm **Nihar Tandra**, a cybersecurity leader based in **Hyderabad, India**, with about **10 years** in security operations and cyber defense.

- 🛡️ **Principal Analyst, Cyber Defense**
- 🔍 Focus areas: threat detection, incident response, SOC operations, and applying AI/ML to security
- 🎓 **MBA**, IIM Lucknow · **B.Tech**, VIT
- 📜 **CISM** · **AWS Certified Cloud Practitioner**

I'm especially interested in where AI meets the SOC: how models triage alerts, how to judge whether "AI-powered detection" actually works, and how to explain all of it to analysts and CISOs without hand-waving. These labs are part of that effort. I built them while learning Python and ML myself, so they're written for people starting from zero.

<!-- Add your links here, e.g.:
📫 [LinkedIn](www.linkedin.com/in/nihar-tandra-83066555) · ✉️ nihartandra4@gmail.com
-->

---

## 🧪 Session 1: Detecting DGA domains

Malware uses **Domain Generation Algorithms (DGAs)** to create thousands of new domains a day to reach its command-and-control server, which makes blocklists useless. This lab teaches a model to spot them.

**The notebook walks through:**
1. Building a sample dataset of normal and DGA domains
2. Turning each domain into numbers (*features*): length, entropy, digit count, vowel ratio, domain age
3. Training a **Random Forest** with the scikit-learn pattern: create → `fit` → `predict`
4. Reading results like a SOC: **confusion matrix, precision, recall**
5. Seeing which features mattered most (domain age drives almost half the decision)
6. Comparing **Decision Tree, KNN, SVM and Random Forest**, with cross-validation
7. Tuning with `GridSearchCV`, saving the model and scoring new domains

**Key results on the sample data:** ~96% precision, ~98% recall.

**The takeaway:** context data the SOC already has, like WHOIS age, beats clever string analysis.

---

## 📡 Session 2: Hunting malicious network beacons

Infected hosts check in with their C2 server on a schedule. The problem is that **EDR heartbeats, NTP, Windows Update and telemetry do too**. This lab shows why a simple "regular traffic" rule drowns analysts in alerts, and how a model that combines several clues does better.

**The notebook walks through:**
1. Generating one day of Zeek-style connection logs: ~370,000 connections from 200 hosts, including tricky look-alikes and stealthy C2 hidden behind cloud IPs
2. A timeline chart that shows the difference between human, malware and legitimate traffic at a glance
3. Using pandas `groupby` to summarise each host → destination pair with 8 analyst-style clues: timing regularity, check-in size, overnight activity and destination rarity
4. Testing the classic beacon rule against the answer key
5. Training a Random Forest and comparing it to the rule on the same hidden test data
6. Producing a **ranked hunt list**, so analysts work from the most suspicious pair down
7. Trying **Isolation Forest** (no labels needed), which is closer to real life
8. *(Optional)* Running the trained model on your own Zeek `conn.log` export

**Key results on the sample data:**

| Approach | C2 caught | False positives | Precision |
|---|---|---|---|
| Classic "regular timing" rule | 34 / 40 | 822 | 4% |
| Random Forest (hidden test set) | 11 / 12 | 0 | 100% |

**The takeaway:** combine clues the way an analyst would, and treat model output as a ranked hunt list rather than a yes/no alert.

---

## 🚀 How to run the labs

**You'll need:** Python 3.9+ and JupyterLab (or Jupyter Notebook, VS Code, or Google Colab).

```bash
# 1. Clone the repo
git clone <this-repo-url>
cd <repo-folder>

# 2. Install the libraries
pip install jupyterlab pandas numpy scikit-learn matplotlib joblib

# 3. Start JupyterLab
jupyter lab
```

Then open a worksheet and press **Shift + Enter** to run each cell, top to bottom. Every cell has a short note above it explaining what it does and what you should see.

**Tips for first-timers**
- Run the cells **in order**. Most beginner errors come from skipping one.
- If something breaks, use **Kernel → Restart Kernel and Run All Cells**.
- The sample data uses a fixed random seed, so your numbers will match the slides exactly.
- Each notebook ends with **experiments to try**, like removing a feature or making the malware noisier. Breaking things on purpose is the fastest way to build intuition.

---

## 🎯 Who this is for

- **SOC analysts and team leads** who want to understand ML without the maths
- **Security managers and customer-facing engineers** who need to ask vendors the right questions about "AI-powered detection"
- **Anyone new to Python** looking for a practical, security-flavoured starting point

---

## ⚠️ Notes

- All data in the notebooks is **synthetic**, generated inside the notebook. No real network or customer data is included.
- The models are **teaching examples**, not production detections. Real deployment needs real labelled data, tuning for your environment and ongoing monitoring.
- If you try the optional step in Session 2 with your own logs, only use data you're **authorised to analyse**, and keep it inside approved systems.

---

## 🙏 Acknowledgements

These labs were inspired by the supervised machine learning module of the AI-for-cybersecurity bootcamp training I attended at **Black Hat India**. The code has been rewritten for current versions of Python and scikit-learn, and the scenarios adapted for SOC teams.

---

⭐ If you find this useful for training your own team, feel free to star the repo or open an issue with feedback.

# Limma Benchmark Suite

Evidence-based web security benchmark suite designed to evaluate real-world detection accuracy with a zero false positive tolerance model.

---

## 🧠 Overview

This benchmark simulates real-world web application behavior using a controlled mock HTTP server and structured test scenarios.

Each scenario includes:

* A predefined **ground truth** (`is_malicious`)
* A fully controlled HTTP response (headers + body)
* An expected detection outcome

This allows precise, reproducible evaluation of security scanning engines.

---

## 🎯 What This Benchmark Covers

The suite includes a wide range of real-world and adversarial scenarios:

* Security headers (CSP, HSTS, X-Frame-Options, etc.)
* Information disclosure (server versions, framework leaks)
* CORS misconfigurations
* CMS fingerprinting (WordPress, etc.)
* Cookie and session security issues
* Redirects, SSRF indicators, internal leaks
* Advanced evasion techniques (header case confusion, obfuscation)
* Encoding attacks (Unicode, Base64, HTML entities)
* Modern vulnerabilities (SSTI, Log4j patterns, deserialization)
* API vulnerabilities (IDOR, JSONP, GraphQL introspection)
* WAF/CDN bypass patterns
* Blind attack indicators (timing, error discrepancies)
* File exposure & path traversal
* JWT weaknesses
* False positive traps (educational content, safe contexts)

---

## ⚙️ Evaluation Model

This benchmark uses an **evidence-driven detection model**:

* Only **Medium, High, and Critical** findings are considered actionable
* Low/Informational findings are treated as noise
* Each detection must be backed by runtime evidence

### Resulting Behavior

* ✅ Zero False Positives (strict validation)
* ⚠️ Controlled False Negatives (intentional tradeoff)

This reflects real-world security priorities where accuracy is preferred over noisy detection.

---

## 🔬 How It Works

1. A mock HTTP server simulates all test endpoints
2. Each endpoint returns a controlled response
3. Limma scans each endpoint via API
4. Results are compared against ground truth (`is_malicious`)
5. Final metrics are calculated:

* True Positive
* False Positive
* True Negative
* False Negative

---

## 🚀 Running the Benchmark

### Requirements

* Node.js
* Limma API running locally

### Start Benchmark

```bash
node benchmark.js
```

### Default Configuration

* Mock server runs on: `http://localhost:9001`
* Limma API endpoint: `http://127.0.0.1:8900/master-report`

---

## 📊 Output

After execution, the following reports are generated:

* `fp_benchmark_report.md` → Human-readable report
* `fp_benchmark_report.csv` → Structured dataset

---

## 🧩 Design Philosophy

This benchmark does not reward aggressive detection.

It prioritizes:

* Verified findings
* Reproducible scenarios
* Real-world accuracy

Instead of asking:

> "Did the scanner find something?"

It asks:

> "Did the scanner find something real?"

---

## 🔓 Transparency

This benchmark is fully open and reproducible.

You can:

* Inspect all scenarios
* Modify test cases
* Run your own comparisons
* Evaluate different tools under identical conditions

---

## ⚠️ Note

Some scenarios are intentionally adversarial and designed to:

* Trigger false positives in weak scanners
* Bypass naive detection logic
* Test normalization and parsing robustness

---

## 🧪 Purpose

This benchmark is designed for:

* Security tool evaluation
* Detection accuracy testing
* Research and experimentation
* Engineering validation of scanning engines

---

## 🏁 Final Thought

Accuracy without evidence is noise.

This benchmark exists to measure the difference.

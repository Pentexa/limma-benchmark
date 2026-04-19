<p align="center">
  <img src="https://img.shields.io/badge/Tests-62_Endpoints-blue?style=for-the-badge" alt="Tests"/>
  <img src="https://img.shields.io/badge/Categories-17-orange?style=for-the-badge" alt="Categories"/>
  <img src="https://img.shields.io/badge/Node.js-≥16-green?style=for-the-badge&logo=node.js" alt="Node"/>
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge" alt="License"/>
  <img src="https://img.shields.io/badge/Zero_Dependencies-✓-purple?style=for-the-badge" alt="Zero Dependencies"/>
</p>

# 🛡️ LİMMA Benchmark Suite

**An open-source, engine-agnostic benchmark for evaluating web vulnerability detection tools.**

How accurate is your security scanner? This benchmark provides a controlled environment with **62 test endpoints** across **17 attack categories** — each with a known ground truth — so you can objectively measure detection accuracy, false positive rates, and blind spots.

---

## 🤔 Why This Benchmark?

Security scanners are only as good as what they catch — and what they _don't_ falsely flag. Without a standardized test suite, it's nearly impossible to compare scanners or track improvements over time.

This benchmark solves that by providing:

- ✅ **Ground-truth labeled endpoints** — Every test case has a known expected result (vulnerable or secure)
- ✅ **Reproducible environment** — A self-contained mock server, no external dependencies
- ✅ **Engine-agnostic design** — Works with any scanner that accepts a URL and returns findings
- ✅ **Comprehensive coverage** — From basic misconfigurations to expert-level evasion techniques
- ✅ **Zero dependencies** — Built entirely on Node.js built-in modules

---

## 📋 Table of Contents

- [Why This Benchmark?](#-why-this-benchmark)
- [Attack Categories](#-attack-categories)
- [Quick Start](#-quick-start)
- [Integrating Your Scanner](#-integrating-your-scanner)
- [How It Works](#-how-it-works)
- [Understanding Results](#-understanding-results)
- [Metrics Explained](#-metrics-explained)
- [Output Reports](#-output-reports)
- [Adding Custom Test Cases](#-adding-custom-test-cases)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🎯 Attack Categories

The suite covers **17 distinct categories**, ranging from basic misconfigurations to expert-level evasion techniques:

| # | Category | Tests | Difficulty |
|---|---|---|---|
| 1 | Perfectly Secure Endpoints | 4 | Baseline |
| 2 | Information Disclosure | 4 | Easy |
| 3 | Security Misconfigurations | 5 | Easy |
| 4 | CORS Misconfigurations | 2 | Medium |
| 5 | CMS Fingerprinting | 1 | Medium |
| 6 | Edge Cases (FP Avoidance) | 2 | Medium |
| 7 | Evasion & Trickery | 5 | Hard |
| 8 | Advanced Encoding & Obfuscation | 4 | Hard |
| 9 | Cookie & Session Security | 4 | Medium |
| 10 | Open Redirect & SSRF | 4 | Hard |
| 11 | Modern Attack Vectors | 5 | Expert |
| 12 | WAF/CDN Bypass | 4 | Expert |
| 13 | API & JSONP Vulnerabilities | 4 | Hard |
| 14 | Blind/Zero-Knowledge Attacks | 4 | Expert |
| 15 | File Upload & Path Traversal | 4 | Hard |
| 16 | False Positive Traps | 5 | Expert |
| 17 | JWT & Token Security | 1 | Medium |

> **Baseline (Secure):** 11 endpoints expected to be clean — tests your scanner's ability to avoid false positives.
>
> **Vulnerable:** 51 endpoints with real vulnerabilities — tests your scanner's detection capability.

---

## 🚀 Quick Start

### Prerequisites

- **Node.js** ≥ 16.x ([Download](https://nodejs.org/))
- **A security scanner** with an HTTP API that accepts URLs to scan

### Step 1: Clone & Enter

```bash
git clone https://github.com/Pentexa/limma-benchmark.git
cd limma-benchmark
```

### Step 2: Configure Your Scanner

Open `fp_benchmark.js` and set your scanner's API endpoint:

```javascript
// Line 5 — Point this to your security scanner's API
const SCANNER_API = "http://127.0.0.1:8900/master-report";
```

### Step 3: Run

```bash
node fp_benchmark.js
```

That's it. No `npm install` needed — zero external dependencies.

---

## 🔌 Integrating Your Scanner

This benchmark is designed to work with **any** security scanner. You only need to modify two things in `fp_benchmark.js`:

### 1. Scanner API Endpoint

```javascript
// Set your scanner's URL analysis endpoint
const SCANNER_API = "http://your-scanner-host:port/analyze";
```

### 2. Response Parsing

The benchmark needs to understand your scanner's output format. Locate the response parsing section (~line 1204) and adapt it to match your scanner's JSON structure:

```javascript
// The benchmark expects to determine:
// - Did the scanner find any security issues? (true/false)
// - What findings were reported? (array of issues)
```

**The contract is simple:**
| Direction | What happens |
|---|---|
| **Benchmark → Scanner** | Sends a URL to analyze |
| **Scanner → Benchmark** | Returns JSON with detected findings |

---

## ⚙️ How It Works

```
┌─────────────────────┐                                  ┌─────────────────────┐
│                     │    POST { url: "http://..." }     │                     │
│   Mock Server       │  ──────────────────────────────▶  │   Your Security     │
│   (Port 9001)       │                                   │   Scanner           │
│                     │  ◀──────────────────────────────  │                     │
│   62 Test Endpoints │    Analysis Results (JSON)        │   (Any Engine)      │
└─────────────────────┘                                   └─────────────────────┘
         ▲                                                         │
         │              HTTP GET (scanner fetches endpoint)         │
         └─────────────────────────────────────────────────────────┘
```

1. **Mock Server** starts on port `9001` with 62 test endpoints — each crafted with specific headers, cookies, and response bodies
2. For each test case, the benchmark sends the target URL to **your scanner's** API
3. Your scanner fetches the mock endpoint, analyzes it, and returns its findings
4. The benchmark compares findings against the **ground truth** label (`is_malicious: true/false`)
5. Results are printed live and saved as Markdown + CSV reports

---

## 📊 Understanding Results

### Live Console Output

Each endpoint is tested and results appear in real-time:

```
[Secure                   ] safe_1_perfect_headers         -> ✅ TRUE NEGATIVE
[Disclosure               ] vuln_1_server_version          -> ✅ TRUE POSITIVE
[Misconfiguration         ] vuln_5_missing_csp             -> ✅ TRUE POSITIVE
[FP Traps                 ] fp_1_security_education_site   -> ✅ TRUE NEGATIVE
[Hardcore (Evasion)       ] hard_1_header_case_confusion   -> ❌ FALSE NEGATIVE (Missed!)
```

### Summary Report

```
================================================================================
                         BENCHMARK FINAL METRICS
================================================================================
Time Elapsed                : 513.83 seconds
Total Endpoints Scanned     : 62
Expected SECURE Targets     : 11
Expected VULN Targets       : 51
--------------------------------------------------------------------------------
✅ True Positives (Found)   : 40
❌ False Positives (Noisy)  : 0
✅ True Negatives (Clean)   : 11
❌ False Negatives (Missed) : 11
--------------------------------------------------------------------------------
🎯 Overall Accuracy         : 82.26%
📢 False Positive Rate      : 0.00%
⚠️  False Negative Rate      : 21.57%
================================================================================
```

---

## 📐 Metrics Explained

| Metric | Formula | What It Tells You |
|---|---|---|
| **True Positive (TP)** | — | Vulnerability correctly detected |
| **False Positive (FP)** | — | Safe endpoint incorrectly flagged as vulnerable |
| **True Negative (TN)** | — | Safe endpoint correctly identified as clean |
| **False Negative (FN)** | — | Vulnerability that was missed |
| **Overall Accuracy** | `(TP + TN) / Total` | General detection reliability |
| **FP Rate** | `FP / (TN + FP)` | How noisy is your scanner? Lower is better |
| **FN Rate** | `FN / (TP + FN)` | How many threats are slipping through? Lower is better |

> **Ideal scanner:** 100% accuracy, 0% FP rate, 0% FN rate.
>
> **In practice:** Most scanners trade off between FP and FN rates — this benchmark helps you see exactly where that trade-off stands.

---

## 📁 Output Reports

After execution, two report files are generated:

| File | Format | Use Case |
|---|---|---|
| `fp_benchmark_report.md` | Markdown | Human-readable breakdown with per-test results and summary metrics |
| `fp_benchmark_report.csv` | CSV | Machine-readable data for custom analysis, charts, or tracking over time |

These files are in `.gitignore` — each user generates their own results.

---

## ➕ Adding Custom Test Cases

Extend the benchmark by adding entries to the `testCases` array in `fp_benchmark.js`:

```javascript
{
    category: "Your Category",
    id: "unique_test_id",
    path: "/your/test/path",
    is_malicious: true,  // true = should be detected, false = should pass clean
    mockResponse: {
        status: 200,
        headers: {
            "Content-Type": "text/html",
            // Add headers relevant to your test scenario
        },
        body: "<html><body>Your crafted response</body></html>"
    }
}
```

**Rules for good test cases:**
- Each `id` and `path` must be unique across the entire suite
- Isolate one vulnerability per test — don't stack multiple issues
- For secure endpoints (`is_malicious: false`), configure all security headers properly
- Add a comment above the test case explaining the expected behavior

---

## 🤝 Contributing

Contributions are welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for full guidelines.

**Ways to contribute:**
- 🐛 **New test cases** — Cover attack vectors not yet in the suite
- 🔌 **Scanner adapters** — Share integration configs for popular scanners
- 📊 **Metrics & reporting** — Improve visualization or add new scoring methods
- 📝 **Documentation** — Fix, improve, or translate

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.

---

<p align="center">
  <sub>Open source security benchmark suite</sub>
</p>

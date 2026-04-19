# Changelog

All notable changes to the Limma Benchmark Suite are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-04-19

### Added

- Initial public release of the Limma Benchmark Suite
- 62 test endpoints across 17 attack categories
- Ground-truth validation framework with binary classification metrics
- Engine-agnostic scanner integration interface
- Comprehensive documentation:
  - `README.md` — Quick start and usage guide
  - `docs/methodology.md` — Evaluation logic and scoring methodology
  - `examples/sample-output.md` — Representative execution example
  - `CONTRIBUTING.md` — Contribution guidelines
- MIT License for open-source distribution

### Test Coverage

- **4** perfectly secure endpoints (false positive tripwires)
- **51** vulnerable endpoints with known security issues
- Categories: Information Disclosure, Misconfigurations, CORS, CMS Fingerprinting, Edge Cases, Evasion, Encoding, Cookies, Redirects, Modern Attacks, WAF Bypass, API Vulnerabilities, Blind Attacks, File/Path Issues, False Positive Traps, JWT Security

### Metrics

- Accuracy calculation: (TP + TN) / Total
- False Positive Rate: FP / (TN + FP)
- False Negative Rate: FN / (TP + FN)
- Severity filtering: Critical, High, Medium only

## Repository Structure

```
limma-benchmark/
├── docs/
│   └── methodology.md        # Evaluation logic documentation
├── examples/
│   └── sample-output.md      # Representative execution output
├── scenarios/
│   └── .gitkeep              # Reserved for future test case separation
├── fp_benchmark.js           # Main benchmark implementation
├── README.md                 # Primary documentation
├── CONTRIBUTING.md           # Contribution guidelines
├── CHANGELOG.md            # Version history
├── LICENSE                   # MIT License
└── package.json              # Package metadata
```

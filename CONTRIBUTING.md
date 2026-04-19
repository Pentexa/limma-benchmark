# Contributing to Security Scanner Benchmark Suite

Thank you for your interest in improving the benchmark! This document provides guidelines for contributing.

## How to Contribute

### 1. Adding New Test Cases

The most valuable contribution is adding new test scenarios. Each test case follows this structure:

```javascript
{
    category: "Category Name",        // Group name for the test
    id: "unique_snake_case_id",       // Unique identifier
    path: "/unique/url/path",         // Mock server endpoint path
    is_malicious: true,               // Ground truth: true = vulnerable, false = secure
    mockResponse: {
        status: 200,                  // HTTP status code
        headers: {
            "Content-Type": "text/html",
            // Add relevant headers for your test scenario
        },
        body: "Response body content"
    }
}
```

**Guidelines for test cases:**

- Each test ID must be unique across the entire suite
- Each path must be unique
- Include a comment explaining *why* this case is malicious or safe
- For `is_malicious: false` (secure) cases, ensure ALL security headers are properly configured
- For `is_malicious: true` (vulnerable) cases, isolate the specific vulnerability — don't stack multiple issues in one test

### 2. Reporting Issues

If you find incorrect ground truth values or engine behavior:

1. Open an issue with the test ID (e.g., `vuln_5_missing_csp`)
2. Describe the expected vs. actual behavior
3. Include your engine version and Node.js version

### 3. Improving Documentation

- Fix typos or unclear explanations
- Add examples for complex attack categories
- Translate documentation to other languages

## Development Setup

```bash
# Clone your fork
git clone https://github.com/YOUR_USERNAME/security-scanner-benchmark.git
cd security-scanner-benchmark

# Run the benchmark (requires a scanner API on the configured port)
node fp_benchmark.js
```

## Pull Request Process

1. Fork the repository
2. Create a feature branch (`git checkout -b feat/new-test-cases`)
3. Make your changes
4. Test locally to ensure the mock server starts correctly
5. Submit a Pull Request with a clear description

## Code Style

- Use 4-space indentation
- Keep test case comments concise but informative
- Follow the existing naming conventions for test IDs:
  - `safe_*` — Secure endpoints
  - `vuln_*` — Vulnerable endpoints
  - `edge_*` — Edge cases
  - `hard_*` — Hardcore/evasion tests
  - `adv_*` — Advanced encoding tests
  - `cookie_*` — Cookie security tests
  - `redirect_*` — Redirect/SSRF tests
  - `modern_*` — Modern attack vectors
  - `waf_*` — WAF/CDN bypass tests
  - `api_*` — API vulnerability tests
  - `blind_*` — Blind/zero-knowledge tests
  - `file_*` — File/path traversal tests
  - `fp_*` — False positive trap tests
  - `jwt_*` — JWT/token security tests

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

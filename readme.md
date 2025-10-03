# 📚 API Testing using Newman

This repository demonstrates **API testing automation** using **Postman collections** and **Newman CLI**, integrated with **GitHub Actions** for continuous testing.
It follows a **Books API** project as the test target.

---

## 🚀 Features

* **Postman → Newman**: Run collections in CI and locally.
* **Docker Compose**: Spins up the Books API before running tests.
* **JUnit Reports**: Test results published in GitHub Actions for visibility.
* **CI Pipeline**: Automated execution on `push` and `workflow_dispatch`.

---

## 📂 Project Structure

```
tests/
├── collections/
│   └── book-app.postman_collection.json   # API tests
├── environments/
│   └── book-env.postman_environment.json # Local env config
└── reports/                              # JUnit/HTML reports
```

---

## 🛠️ Running Locally

### 1. Clone and install dependencies

```bash
git clone https://github.com/hossainks/postman-newman-tests.git
cd postman-newman-tests
npm install -g newman
```

### 2. Start the backend service

If your Books API is dockerized:

```bash
docker compose up -d --build
```

Confirm health endpoint:

```bash
curl http://localhost:3000/health
```

### 3. Run Newman tests

```bash
npx newman run tests/collections/book-app.postman_collection.json \
  -e tests/environments/book-env.postman_environment.json \
  -r cli,junit \
  --reporter-junit-export tests/reports/results.xml
```

📊 Reports:

* **CLI** → Printed in terminal
* **HTML** → `tests/reports/report.html`
* **JUnit** → `tests/reports/results.xml`

---

## ⚡ CI/CD Workflow

GitHub Actions workflow:

* Spins up backend service with **Docker Compose**
* Runs **Newman tests** with reporters
* Publishes results with [`dorny/test-reporter`](https://github.com/dorny/test-reporter)

Example workflow snippet:

```yaml
- name: Run API tests with Newman
  run: |
    npx newman run tests/collections/book-app.postman_collection.json \
    -e tests/environments/book-env.postman_environment.json \
    -r cli,junit \
    --reporter-junit-export tests/reports/results.xml
```

---

## ✅ Sample Test Report (Latest Run)

* **31 tests executed**
* **31 passed, 0 failed, 0 skipped**
* **Execution time: 246ms**

| Test Suite                     | Status   |
| ------------------------------ | -------- |
| Health                         | ✅ Passed |
| Register                       | ✅ Passed |
| Login                          | ✅ Passed |
| Get All Books                  | ✅ Passed |
| Get Book by ID                 | ✅ Passed |
| Create / Update / Delete Books | ✅ Passed |

---

## 🎯 Key Takeaways

* Demonstrates **real-world API testing strategy** (Postman + Newman).
* Integrates testing into **CI/CD pipelines** for fast feedback.
* Provides **structured reporting** for traceability.
* Designed to be **scalable** for microservices or larger APIs.
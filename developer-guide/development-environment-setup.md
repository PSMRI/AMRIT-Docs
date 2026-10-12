## 🚀 First-Time Backend Contributor Guide (Windows + IntelliJ)

This section provides additional clarity for first-time contributors setting up AMRIT backend repositories (e.g., Common-API, HWC-API) on Windows using IntelliJ IDEA.

These notes are based on real onboarding experience and aim to reduce common setup confusion.

---

### 1️⃣ Repository Selection – Which Repo Should I Work In?

AMRIT consists of multiple repositories (e.g., AMRIT, Common-API, HWC-API, Identity-API, etc.).

If you are contributing to backend services:

- Identify the correct service-line repository (e.g., `Common-API`, `HWC-API`)
- Do **not** directly work in the umbrella/meta repository unless required
- Confirm via issue labels (Common-API / HWC-API / etc.) which repo needs changes

💡 Suggestion: A small decision tree in documentation could help contributors quickly identify the correct repository.

---

### 2️⃣ Required Java Version & IntelliJ Configuration

If you see:


Project JDK is not defined


Follow these steps:

- Install Java (Recommended: Java 17)
- In IntelliJ:
  - Go to **File → Project Structure**
  - Set **Project SDK** to installed Java version
  - Set language level accordingly

Without configuring Project SDK, the project may show red errors even if dependencies are correct.

---

### 3️⃣ Maven Build Output – Understanding `BUILD SUCCESS`

When running:


mvn clean install


You may see:


BUILD SUCCESS
Tests run: 1778, Failures: 43


This can be confusing.

Clarification:

- `BUILD SUCCESS` means the project compiled successfully.
- Test failures may still be present.
- For documentation-only or small changes, failing legacy tests may not always block progress.
- Always verify whether test failures are related to your changes before proceeding.

💡 Suggestion: Add a short explanation in setup docs about how to interpret Maven output.

---

### 4️⃣ Database Schema & Flyway Context

When modifying entities or adding new fields:

- Verify whether a database column already exists.
- Check Flyway migration scripts for schema changes.
- Avoid modifying entity classes without understanding DB alignment.

Suggested Documentation Addition:

- Brief explanation of:
  - Where Flyway migrations are located
  - When DB migration is required
  - How to validate schema consistency

This would prevent confusion when adding new columns (e.g., `lock_until`) or updating entities.

---

### 5️⃣ Git Workflow for First-Time Contributors

If you encounter:


Permission denied (403) while pushing


This usually means you are trying to push directly to the upstream repository.

Correct workflow:

1. Fork the repository
2. Clone your fork
3. Create a feature branch:

git checkout -b feature/your-change

4. Push to your fork:

git push origin feature/your-change

5. Create a Pull Request to upstream repository

💡 Suggestion: Add a “First Contribution Git Workflow” section in documentation.

---

### 6️⃣ Recommended Backend Setup Checklist

Before starting development:

- ✅ Install Java (Recommended: 17)
- ✅ Install Maven
- ✅ Configure Project SDK in IntelliJ
- ✅ Clone your fork (not upstream directly)
- ✅ Run `mvn clean install`
- ✅ Confirm application starts successfully
- ✅ Understand which repository you are modifying
- ✅ Create a feature branch before making changes

---

### 🎯 Summary

These additions would significantly reduce onboarding friction for:

- First-time contributors
- Students contributing via C4GT / SheCodes
- Developers unfamiliar with Spring Boot + Maven workflows

Improving clarity around repository selection, SDK setup, Maven output interpretation, database alignment, and Git workflow would make AMRIT much more accessible to new contributors.
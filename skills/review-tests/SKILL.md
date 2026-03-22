---
name: review-tests
description: Systematic test suite review — structure, DRY, assertion quality, dead tests, fixture hygiene. Use when the user asks to review, audit, or improve tests.
---

# Systematic Test Review

You are performing a systematic review of the test suite, analysing structure, fixture hygiene, assertion quality, and DRY adherence. Work through the phases below in order.

---

## Phase 1: Discovery

### 1. Map Structure

Compare test directory layout against source layout:

```
src/project/graph/       → tests/graph/
src/project/agent/       → tests/agent/
src/project/utils/       → tests/utils/
```

Flag:
- Flat test directories that don't mirror source structure
- Test files that can't be mapped to a source module
- Missing `conftest.py` in test subdirectories
- Missing `__init__.py` where needed for test discovery

### 2. Inventory

For each test file, record:
- File path and line count
- Number of test classes and test methods
- Which source module it tests (by inspecting imports)
- Whether a `conftest.py` exists in its directory

### 3. Produce Analysis Plan

Write `test-review/00-analysis-plan.md` containing:

- **Structure assessment** — Does test layout mirror source? Which directories are missing?
- **File inventory table** — path, LOC, test count, source module, priority
- **Priority criteria**: High = tests for core business logic, security, data integrity; Medium = tests for integrations, utilities; Low = tests for config, scripts, types
- **Known risks** — Large files (>500 lines), files with 0 assertions, files with many skips

---

## Phase 2: Review Dimensions

Review each test file against these dimensions. Focus on actionable issues, not nitpicks.

### 1. DRY / Fixture Hygiene

| Smell | Threshold | Action |
|-------|-----------|--------|
| Same fixture defined in 2+ files | Any duplication | Extract to nearest shared `conftest.py` |
| Same mock construction in 3+ test methods | 3+ repetitions | Extract to fixture or factory helper |
| Same setup/teardown across test classes | 2+ classes | Shared fixture with appropriate scope |
| Repeated lambda, constant, or builder | 3+ sites | Module-level constant or helper function |
| Fixture only used by one test | Single consumer | Inline into the test or make intent clearer |

When extracting fixtures to `conftest.py`:
- Place at the **narrowest shared scope** (subdirectory conftest, not root)
- Keep fixtures that are only relevant to one file **in that file**
- Use `@pytest.fixture(scope="module")` or `scope="session"` only when setup is expensive and state is truly immutable

### 2. Assertion Quality

**Red flags — always fix:**

| Pattern | Problem | Fix |
|---------|---------|-----|
| `assert x is not None` alone | Doesn't verify what x contains | Follow with content assertions |
| `assert isinstance(x, list)` | Guaranteed by return type | Assert contents or length |
| `assert len(x) > 0` | Doesn't verify expected count | Assert specific count when known |
| `if result:` before assertions | Silently passes when result is None | `assert result is not None` then assert properties |
| No assertion after operation | Tests "doesn't crash" not behavior | Add assertions for expected outcome |
| `assert True` / `assert 1` | Meaningless | Delete or replace with real assertion |

**Acceptable — don't flag:**

- `assert x is not None` followed by property assertions (guard + verify pattern)
- `assert len(x) >= N` when exact count is non-deterministic
- `with pytest.raises(XError):` without message check (exception type is often sufficient)

### 3. Parametrization Opportunities

| Signal | Action |
|--------|--------|
| 3+ test methods that differ only by input values | `@pytest.mark.parametrize` |
| Loop inside a test method testing multiple cases | Parametrize (each case gets its own pass/fail) |
| Test class with methods like `test_X_with_A`, `test_X_with_B`, `test_X_with_C` | Parametrize if setup is identical |

**Don't parametrize when:**
- Tests have different setup or teardown requirements
- The readability cost outweighs the DRY benefit
- Test names would become meaningless (`test_case[0]`, `test_case[1]`)

### 4. Dead Weight

| Pattern | Action |
|---------|--------|
| `@pytest.mark.skip` without linked issue or clear unblock condition | Fix or delete |
| `pass` body test methods | Delete |
| Commented-out test code | Delete |
| Tests for removed/renamed features (imports fail or mock nonexistent classes) | Delete |
| Unused fixtures (defined but never referenced) | Delete |
| Empty test files or files with only imports | Delete |

### 5. Test Independence

| Problem | Signal | Fix |
|---------|--------|-----|
| Order-dependent tests | Tests fail when run individually but pass in suite | Remove shared mutable state |
| Leaked state | Tests fail on second run or with `--count=2` | Ensure fixture cleanup (yield + teardown) |
| File system pollution | Tests create files in working directory | Use `tmp_path` fixture |
| Port/resource conflicts | Tests fail when run in parallel | Use dynamic ports or mock I/O |

### 6. Error Path Coverage

For each source module's error handling (try/except, if/else guards, validation):
- Is the error path tested?
- Does the test verify the **specific behavior** on error (return value, exception type, logged message)?
- Not just "doesn't crash"

---

## Phase 3: Output Structure

```
test-review/
├── 00-analysis-plan.md           # Structure assessment and file inventory
├── 00-summary.md                 # Key findings, scores, action priorities
├── findings/
│   ├── structure.md              # Layout issues, missing conftest files
│   ├── dry-violations.md         # Duplicated fixtures, copy-pasted tests
│   ├── assertion-quality.md      # Weak assertions, missing assertions
│   ├── dead-weight.md            # Skipped tests, dead code, unused fixtures
│   └── parametrization.md        # Opportunities to consolidate tests
└── recommendations/
    ├── conftest-extractions.md   # Which fixtures to extract where (with code)
    ├── quick-fixes.md            # Single-line assertion improvements
    └── refactoring.md            # Larger structural changes
```

### Finding Format

Each finding must include:

- **File:line** — exact location
- **Category** — DRY, Assertion, Dead Weight, Parametrization, Independence
- **Current code** — the problematic snippet
- **Recommended fix** — concrete replacement code
- **Impact** — what improves (coverage confidence, maintainability, test count)

---

## Phase 4: Fix Prioritization

### Do First (highest value, lowest risk)

1. **Create missing `conftest.py` files** and extract duplicated fixtures
2. **Delete dead tests** — skipped stubs, empty bodies, commented-out code
3. **Fix silent pass-through** — `if result:` guards before assertions

### Do Next

4. **Strengthen weak assertions** — add content checks after existence checks
5. **Parametrize duplicate tests** — 3+ methods with same structure, different inputs
6. **Add missing error path assertions** — tests that verify "no crash" but not behavior

### Do Later

7. **Restructure test directories** to mirror source layout (if not already done)
8. **Consolidate overlapping test files** that test the same module at different levels
9. **Add missing edge case tests** identified during review

---

## Scoring

Score the test suite 1-10 on each dimension:

| Dimension | Weight | What 8+ looks like | What 4- looks like |
|-----------|--------|--------------------|--------------------|
| Assertion Quality | 30% | Every test verifies specific behavior with concrete expected values. No silent pass-through, no `assert True`. | Tests check existence only (`is not None`, `isinstance`). `if result:` guards hide failures. Operations without any assertion. |
| DRY / Fixtures | 20% | Shared fixtures in conftest at the right scope. No fixture defined in more than one file. Helpers extracted at 3+ repetitions. | Same mock construction copy-pasted across 5+ test methods. Identical fixtures in multiple files. Repeated lambdas and builders. |
| Error Path Coverage | 18% | Every try/except and validation branch in source has a test that verifies the specific error behavior (return value, exception type, side effect). | Error tests only verify "doesn't crash." Missing tests for exception handlers, fallback paths, and validation rejects. |
| Structure | 15% | Test directories mirror source. Every subdirectory has conftest.py if it shares fixtures. File names clearly map to source modules. | Flat directory with 40+ files. No conftest files. Test names don't indicate what they cover. |
| Dead Weight | 10% | Zero skipped tests without linked issues. No stub bodies, no commented-out tests, no unused fixtures. | Multiple `@skip` with no resolution path. Empty `pass` test methods. Commented-out test blocks. Files with 0 assertions. |
| Independence | 7% | Every test passes when run in isolation (`pytest path/to/test.py::test_name`). No leaked state, temp files cleaned up, no port conflicts. | Tests fail when run individually. Shared mutable module state. Files created in working directory. Flaky under parallel execution. |

---

## What NOT to Flag

- **Test verbosity** — Tests should be readable, not minimized. Three clear lines beat one clever one.
- **Missing docstrings on test methods** — The test name should be the documentation.
- **Style preferences** — Don't reformat working tests for aesthetics.
- **Missing edge cases for trivial code** — Focus review time on business logic.
- **Mocking strategy disagreements** — Unless mocks are actively hiding bugs, leave the approach alone.

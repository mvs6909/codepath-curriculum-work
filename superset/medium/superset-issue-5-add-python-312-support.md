## Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/apache/superset/33434)
- **PR:** [apache/superset#33434](https://github.com/apache/superset/pull/33434)
- **Issue:** N/A

# Add Python 3.12 support

## Motivation

Superset currently supports Python 3.10 and 3.11, but does not support Python 3.12, which was released in October 2023. As Python 3.12 becomes more widely adopted and organizations upgrade their Python environments, Superset needs to ensure compatibility with this version to remain accessible to users and avoid blocking upgrades.

Supporting Python 3.12 requires addressing breaking changes in dependencies and APIs that have evolved between Python versions. This includes updating dependency constraints to versions compatible with Python 3.12, adapting to deprecated API patterns (particularly in pandas and SQLAlchemy), and ensuring the CI/CD pipeline validates compatibility across all supported Python versions. This work ensures Superset remains modern and maintainable as the Python ecosystem evolves.

## Current Behavior

Superset cannot run on Python 3.12 due to incompatible dependency versions and deprecated API usage. The project's CI/CD workflows only test against "current" and "previous" Python versions (3.11 and 3.10), and the package metadata does not declare Python 3.12 support.

**Reproduction Steps:**

1. Set up a Python 3.12 environment
2. Attempt to install Superset dependencies using the current `requirements/base.txt` and `pyproject.toml`
3. Observe: Installation fails or produces warnings due to incompatible dependency versions (numpy, pandas, tabulate)
4. If installation succeeds, attempt to run Superset and execute database queries that use pandas
5. Observe: Runtime errors occur due to deprecated pandas API usage (specifically `pd.read_sql_query` without proper connection context)
6. Check CI/CD workflows in `.github/workflows/`
7. Observe: No Python 3.12 testing is configured in the test matrix

## Expected Behavior

Superset should fully support Python 3.12 alongside existing Python 3.10 and 3.11 support. All dependencies should resolve correctly, the application should run without errors, and CI/CD pipelines should validate compatibility with Python 3.12.

**Acceptance Criteria:**

- [ ] Python 3.12 is declared as a supported version in `pyproject.toml` classifiers
- [ ] All dependency constraints are updated to versions compatible with Python 3.12 (numpy, pandas, tabulate, and any transitive dependencies)
- [ ] Code using deprecated pandas APIs is updated to use current best practices (proper connection context management)
- [ ] CI/CD workflows (pre-commit, unit tests, integration tests) include Python 3.12 in their test matrix
- [ ] All existing tests pass on Python 3.12 without errors or warnings related to version incompatibility

## Verification

**Manual Testing:**
1. Create a fresh Python 3.12 virtual environment
2. Install Superset from the updated requirements: `pip install -r requirements/base.txt`
3. Verify installation completes without errors
4. Start Superset and navigate to a chart that queries a database
5. Verify the chart renders successfully without pandas-related errors
6. Check that database query operations complete successfully

**Automated Testing:**
1. Run the pre-commit workflow: observe that it executes against Python 3.12
2. Run unit tests: `pytest tests/unit_tests/` on Python 3.12 environment
3. Run integration tests: `pytest tests/integration_tests/` on Python 3.12 environment
4. Verify all test suites pass with the same success rate as Python 3.11
5. Check CI/CD pipeline runs and confirms all three Python versions (3.10, 3.11, 3.12) pass

**Dependency Verification:**
1. Run `pip check` in a Python 3.12 environment to ensure no dependency conflicts
2. Verify that `numpy`, `pandas`, and `tabulate` versions are compatible with Python 3.12
3. Confirm no deprecation warnings appear during test execution related to pandas or SQLAlchemy usage


### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx
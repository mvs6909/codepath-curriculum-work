## Metadata

- **Tool:** [OpenBootstrap](https://openbootstrap.onrender.com/pr/apache/superset/33929)
- **PR:** [apache/superset#33929](https://github.com/apache/superset/pull/33929)
- **Issue:** N/A

# Remove Rockset database support from Superset

## Motivation

Rockset was acquired by OpenAI in 2024, and their online service was shut down on September 30, 2024. The service is no longer available to customers, and remaining users have likely migrated to alternative database solutions. Maintaining support for a defunct database service adds unnecessary maintenance burden, confuses users reviewing supported databases, and keeps obsolete dependencies in the project.

Removing deprecated database support is an important maintenance task that keeps the codebase clean, reduces confusion for new users evaluating Superset's capabilities, and eliminates technical debt. This change will affect documentation, configuration files, database engine specifications, and test suites.

## Current Behavior

Superset currently includes full support for Rockset as a database backend, including:
- Database engine specification implementation
- Documentation on how to connect to Rockset
- Rockset logo and branding in the README
- Python package dependency for the Rockset SQLAlchemy driver
- Unit tests for Rockset-specific functionality
- References in database support matrices and configuration guides

**Reproduction Steps:**
1. Open the project README.md and search for "rockset" - observe the Rockset logo is displayed among supported databases
2. Check `docs/docs/configuration/databases.mdx` - observe there is a dedicated section explaining how to connect to Rockset
3. Review `pyproject.toml` - observe `rockset-sqlalchemy` is listed as an optional dependency
4. Examine `superset/db_engine_specs/rockset.py` - observe a complete database engine specification exists for Rockset
5. Check `tests/unit_tests/db_engine_specs/test_rockset.py` - observe unit tests exist for Rockset functionality
6. Review `superset/sql/parse.py` - observe commented reference to Rockset in dialect mapping
7. Observe: Rockset support is fully integrated throughout the codebase despite the service being shut down

## Expected Behavior

Superset should have all Rockset-related code, documentation, and configuration removed since the service is no longer operational. Users should not see Rockset listed as a supported database, and the codebase should not contain any references to the defunct service.

**Acceptance Criteria:**
- [ ] Rockset logo and references removed from README.md
- [ ] Rockset documentation section removed from database configuration guide
- [ ] Rockset SQLAlchemy dependency removed from pyproject.toml
- [ ] Rockset database engine specification file deleted
- [ ] Rockset unit tests deleted
- [ ] Any remaining Rockset references cleaned up from code comments and documentation
- [ ] Database support matrix updated to exclude Rockset

## Verification

**Manual Verification:**
1. Search the entire codebase for "rockset" (case-insensitive) - should find no meaningful references
2. Review the README.md - confirm Rockset logo is not displayed among supported databases
3. Check the database configuration documentation - confirm no Rockset connection instructions exist
4. Review `pyproject.toml` - confirm no `rockset-sqlalchemy` dependency is listed

**Automated Verification:**
1. Run the test suite to ensure no tests are broken by the removal: `pytest tests/`
2. Verify the project builds successfully without the Rockset dependency
3. Check that documentation builds without errors: `cd docs && npm run build` (if applicable)
4. Run linting to ensure code quality is maintained: `pre-commit run --all-files`

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

Guide to submitting pull requests: https://hackmd.io/@timothy1ee/Hky8kV3hlx


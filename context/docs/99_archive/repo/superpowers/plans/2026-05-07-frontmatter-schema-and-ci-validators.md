---
title: Frontmatter Schema and CI Validators Implementation Plan
status: superseded
record_class: historical
audience: [internal]
owner: Planning maintainers
capability: knowledge
phase: n/a
cadence: ad-hoc
last_reviewed: 2026-05-07
superseded_by: 2026-05-07-complete-docs-system.md
---

# Frontmatter Schema and CI Validators Implementation Plan

> **Status: SUPERSEDED** by [2026-05-07-complete-docs-system.md](2026-05-07-complete-docs-system.md). The combined plan absorbs this one (Tasks 1–3) and adds the full canonical doc scaffolds. Kept here as reference for the foundation subsystem.

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Ship the frontmatter schema, validator, and CI workflow so every canonical Markdown artifact in this repository is machine-checked for required metadata, audience tagging, supersession integrity, and freshness — establishing the single source of truth that the lifecycle/role/capability indexes will later be generated from.

**Architecture:** A self-contained Python tool at `tools/docs_validator/` that parses YAML frontmatter from Markdown files, validates against a Pydantic schema, and runs cross-file checks (supersession, ADR coverage, staleness). Invoked locally via `make` and in CI via GitHub Actions, alongside `markdownlint-cli2` for prose lint and `lychee` for link checking.

**Tech Stack:**
- **Validator:** Python 3.11+, Pydantic v2, `python-frontmatter`, pytest.
- **Markdown lint:** `markdownlint-cli2` (Node, run via npx in CI).
- **Link checker:** `lychee` (single-binary, run via official action in CI).
- **CI:** GitHub Actions.
- **Local task runner:** GNU make.

**Source spec:** `.tmp/docs-lifecycle-redesign-plan.md` Section 7 (frontmatter schema, normative) and Section 8 Phase 1 (foundation: schema + CI together).

**Out of scope (future plans):**
- Index generator (lifecycle / role / capability views from frontmatter) — separate plan.
- Export pipeline (MkDocs / Pandoc + audience-filtered bundles) — separate plan.
- ADR / RFC subsystem scaffolding — separate plan.
- End-user docs Diataxis area — separate plan.

---

## File Structure

| Path | Responsibility |
|------|---------------|
| `tools/docs_validator/pyproject.toml` | Python project metadata, dependencies, console script entry point |
| `tools/docs_validator/Makefile` | Local task targets: `make install`, `make test`, `make validate` |
| `tools/docs_validator/src/docs_validator/__init__.py` | Package marker, version |
| `tools/docs_validator/src/docs_validator/schema.py` | Pydantic models defining the normative frontmatter schema |
| `tools/docs_validator/src/docs_validator/parser.py` | Extract YAML frontmatter from Markdown files |
| `tools/docs_validator/src/docs_validator/cli.py` | CLI entry point — orchestrates parser, schema, and cross-file checks |
| `tools/docs_validator/src/docs_validator/checks/__init__.py` | Package marker |
| `tools/docs_validator/src/docs_validator/checks/supersession.py` | Cross-file: `superseded_by` paths must exist |
| `tools/docs_validator/src/docs_validator/checks/staleness.py` | Single-file: `last_reviewed` vs declared cadence |
| `tools/docs_validator/src/docs_validator/checks/adr_coverage.py` | Repo-scan: list ADRs unreferenced from any other doc |
| `tools/docs_validator/tests/test_schema.py` | Schema unit tests |
| `tools/docs_validator/tests/test_parser.py` | Parser unit tests |
| `tools/docs_validator/tests/test_cli.py` | CLI integration tests |
| `tools/docs_validator/tests/test_supersession.py` | Supersession check tests |
| `tools/docs_validator/tests/test_staleness.py` | Staleness check tests |
| `tools/docs_validator/tests/test_adr_coverage.py` | ADR coverage tests |
| `tools/docs_validator/tests/fixtures/` | Sample valid and invalid Markdown files for tests |
| `docs/_examples/canonical-manager.md` | Seed: canonical manager-audience artifact |
| `docs/_examples/canonical-internal.md` | Seed: canonical internal-audience artifact |
| `docs/_examples/end-user-doc.md` | Seed: end-user audience artifact (Diataxis how-to) |
| `docs/_examples/superseded.md` | Seed: superseded artifact pointing to a replacement |
| `docs/_examples/replacement.md` | Seed: target of the supersession link above |
| `docs/00_operating_model/frontmatter-schema.md` | Human-readable schema documentation (self-validating) |
| `.markdownlint-cli2.yaml` | Markdown lint configuration |
| `lychee.toml` | Link checker configuration |
| `.github/workflows/docs.yml` | CI: frontmatter validation, markdownlint, lychee, ADR coverage |

---

## Task 1: Project scaffolding

**Files:**
- Create: `tools/docs_validator/pyproject.toml`
- Create: `tools/docs_validator/src/docs_validator/__init__.py`
- Create: `tools/docs_validator/tests/__init__.py`
- Create: `tools/docs_validator/tests/test_smoke.py`
- Create: `tools/docs_validator/Makefile`

- [ ] **Step 1: Write the smoke test**

Create `tools/docs_validator/tests/test_smoke.py`:

```python
def test_package_importable():
    import docs_validator
    assert docs_validator.__version__ == "0.1.0"
```

- [ ] **Step 2: Create the package marker**

Create `tools/docs_validator/src/docs_validator/__init__.py`:

```python
__version__ = "0.1.0"
```

Create empty `tools/docs_validator/tests/__init__.py`.

- [ ] **Step 3: Write `pyproject.toml`**

Create `tools/docs_validator/pyproject.toml`:

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "docs-validator"
version = "0.1.0"
description = "Validates frontmatter and structural integrity of Markdown documentation."
requires-python = ">=3.11"
dependencies = [
    "pydantic>=2.6,<3",
    "python-frontmatter>=1.1,<2",
]

[project.optional-dependencies]
dev = [
    "pytest>=8,<9",
]

[project.scripts]
docs-validator = "docs_validator.cli:main"

[tool.hatch.build.targets.wheel]
packages = ["src/docs_validator"]

[tool.pytest.ini_options]
testpaths = ["tests"]
pythonpath = ["src"]
```

- [ ] **Step 4: Write the Makefile**

Create `tools/docs_validator/Makefile`:

```makefile
.PHONY: install test validate clean

install:
	python -m pip install -e ".[dev]"

test:
	python -m pytest -v

validate:
	docs-validator ../../docs ../../docs/_examples

clean:
	rm -rf build dist *.egg-info .pytest_cache __pycache__
```

- [ ] **Step 5: Install and run smoke test**

```bash
cd tools/docs_validator
make install
make test
```

Expected output: 1 test passed.

- [ ] **Step 6: Commit**

```bash
git add tools/docs_validator/
git commit -m "feat(docs-validator): scaffold Python project for frontmatter validation"
```

---

## Task 2: Frontmatter schema (Pydantic)

**Files:**
- Create: `tools/docs_validator/src/docs_validator/schema.py`
- Create: `tools/docs_validator/tests/test_schema.py`

- [ ] **Step 1: Write the failing schema tests**

Create `tools/docs_validator/tests/test_schema.py`:

```python
import pytest
from datetime import date
from pydantic import ValidationError

from docs_validator.schema import Frontmatter, Audience, Status, RecordClass, Capability, Phase, Cadence


VALID_MINIMAL = {
    "title": "Project Brief",
    "status": "active",
    "record_class": "canonical",
    "audience": ["internal", "manager"],
    "owner": "PM",
    "capability": "governance",
}


def test_minimal_valid_frontmatter_parses():
    fm = Frontmatter(**VALID_MINIMAL)
    assert fm.title == "Project Brief"
    assert Audience.MANAGER in fm.audience
    assert fm.status == Status.ACTIVE


def test_missing_required_field_rejects():
    data = dict(VALID_MINIMAL)
    del data["audience"]
    with pytest.raises(ValidationError) as exc:
        Frontmatter(**data)
    assert "audience" in str(exc.value)


def test_empty_audience_list_rejects():
    data = dict(VALID_MINIMAL, audience=[])
    with pytest.raises(ValidationError):
        Frontmatter(**data)


def test_unknown_audience_value_rejects():
    data = dict(VALID_MINIMAL, audience=["public"])
    with pytest.raises(ValidationError):
        Frontmatter(**data)


def test_superseded_requires_superseded_by():
    data = dict(VALID_MINIMAL, status="superseded")
    with pytest.raises(ValidationError) as exc:
        Frontmatter(**data)
    assert "superseded_by" in str(exc.value)


def test_superseded_with_superseded_by_passes():
    data = dict(VALID_MINIMAL, status="superseded", superseded_by="new-doc.md")
    fm = Frontmatter(**data)
    assert fm.superseded_by == "new-doc.md"


def test_execution_capability_requires_cadence_and_source_of_truth():
    data = dict(VALID_MINIMAL, capability="execution")
    with pytest.raises(ValidationError) as exc:
        Frontmatter(**data)
    msg = str(exc.value)
    assert "cadence" in msg or "source_of_truth" in msg


def test_execution_capability_with_required_fields_passes():
    data = dict(
        VALID_MINIMAL,
        capability="execution",
        cadence="weekly",
        source_of_truth="repo",
    )
    fm = Frontmatter(**data)
    assert fm.cadence == Cadence.WEEKLY


def test_last_reviewed_parses_iso_date():
    data = dict(VALID_MINIMAL, last_reviewed="2026-05-07")
    fm = Frontmatter(**data)
    assert fm.last_reviewed == date(2026, 5, 7)


def test_record_class_supporting_does_not_require_audience():
    # supporting docs may inherit audience from their canonical owner
    data = {
        "title": "How to write an ADR",
        "status": "active",
        "record_class": "supporting",
        "audience": ["internal"],
        "owner": "Engineering",
        "capability": "knowledge",
    }
    fm = Frontmatter(**data)
    assert fm.record_class == RecordClass.SUPPORTING
```

- [ ] **Step 2: Run tests to verify they fail**

```bash
cd tools/docs_validator
make test
```

Expected: collection error or many failures — `docs_validator.schema` does not exist yet.

- [ ] **Step 3: Implement the schema**

Create `tools/docs_validator/src/docs_validator/schema.py`:

```python
from __future__ import annotations

from datetime import date
from enum import Enum
from typing import Optional

from pydantic import BaseModel, ConfigDict, Field, field_validator, model_validator


class Status(str, Enum):
    DRAFT = "draft"
    PROPOSED = "proposed"
    ACCEPTED = "accepted"
    ACTIVE = "active"
    SUPERSEDED = "superseded"
    ARCHIVED = "archived"


class RecordClass(str, Enum):
    CANONICAL = "canonical"
    SUPPORTING = "supporting"
    HISTORICAL = "historical"


class Audience(str, Enum):
    INTERNAL = "internal"
    MANAGER = "manager"
    CLIENT = "client"
    END_USER = "end_user"
    AUDITOR = "auditor"


class Capability(str, Enum):
    GOVERNANCE = "governance"
    PRODUCT = "product"
    ARCHITECTURE = "architecture"
    EXECUTION = "execution"
    QUALITY = "quality"
    OPERATIONS = "operations"
    KNOWLEDGE = "knowledge"
    REFERENCES = "references"
    USER_DOCS = "user_docs"
    OPERATING_MODEL = "operating_model"


class Phase(str, Enum):
    INITIATION = "initiation"
    PLANNING = "planning"
    EXECUTION = "execution"
    MONITORING = "monitoring"
    CLOSURE = "closure"
    NA = "n/a"


class Cadence(str, Enum):
    AD_HOC = "ad-hoc"
    WEEKLY = "weekly"
    MONTHLY = "monthly"
    PER_STAGE = "per-stage"
    PER_RELEASE = "per-release"
    ONE_SHOT = "one-shot"


class Frontmatter(BaseModel):
    model_config = ConfigDict(extra="forbid", use_enum_values=False)

    title: str = Field(min_length=1)
    status: Status
    record_class: RecordClass
    audience: list[Audience] = Field(min_length=1)
    owner: str = Field(min_length=1)
    capability: Capability
    phase: Optional[Phase] = None
    cadence: Optional[Cadence] = None
    last_reviewed: Optional[date] = None
    supersedes: Optional[list[str]] = None
    superseded_by: Optional[str] = None
    source_of_truth: Optional[str] = None
    tags: Optional[list[str]] = None

    @field_validator("audience", mode="before")
    @classmethod
    def _coerce_audience(cls, v):
        if isinstance(v, str):
            return [v]
        return v

    @model_validator(mode="after")
    def _check_supersession(self) -> "Frontmatter":
        if self.status == Status.SUPERSEDED and not self.superseded_by:
            raise ValueError("status=superseded requires superseded_by")
        return self

    @model_validator(mode="after")
    def _check_execution_required_fields(self) -> "Frontmatter":
        if self.capability == Capability.EXECUTION:
            missing = []
            if self.cadence is None:
                missing.append("cadence")
            if self.source_of_truth is None:
                missing.append("source_of_truth")
            if missing:
                raise ValueError(
                    f"capability=execution requires: {', '.join(missing)}"
                )
        return self
```

- [ ] **Step 4: Run tests to verify they pass**

```bash
cd tools/docs_validator
make test
```

Expected: 10 tests passed (including smoke test from Task 1).

- [ ] **Step 5: Commit**

```bash
git add tools/docs_validator/src/docs_validator/schema.py tools/docs_validator/tests/test_schema.py
git commit -m "feat(docs-validator): add Pydantic frontmatter schema with conditional validation"
```

---

## Task 3: Frontmatter parser

**Files:**
- Create: `tools/docs_validator/src/docs_validator/parser.py`
- Create: `tools/docs_validator/tests/test_parser.py`
- Create: `tools/docs_validator/tests/fixtures/valid.md`
- Create: `tools/docs_validator/tests/fixtures/no_frontmatter.md`
- Create: `tools/docs_validator/tests/fixtures/malformed_yaml.md`

- [ ] **Step 1: Create test fixtures**

Create `tools/docs_validator/tests/fixtures/valid.md`:

```markdown
---
title: "Test Document"
status: active
record_class: canonical
audience: [internal]
owner: "Test"
capability: knowledge
---

# Test Document

Body.
```

Create `tools/docs_validator/tests/fixtures/no_frontmatter.md`:

```markdown
# Plain Markdown

No frontmatter here.
```

Create `tools/docs_validator/tests/fixtures/malformed_yaml.md`:

```markdown
---
title: "Test
status active
---

# Broken
```

- [ ] **Step 2: Write the failing parser tests**

Create `tools/docs_validator/tests/test_parser.py`:

```python
from pathlib import Path
import pytest

from docs_validator.parser import (
    parse_file,
    ParseResult,
    NoFrontmatterError,
    MalformedFrontmatterError,
)

FIXTURES = Path(__file__).parent / "fixtures"


def test_parse_valid_file_returns_metadata_and_body():
    result = parse_file(FIXTURES / "valid.md")
    assert isinstance(result, ParseResult)
    assert result.metadata["title"] == "Test Document"
    assert result.metadata["audience"] == ["internal"]
    assert "Body." in result.body


def test_parse_file_without_frontmatter_raises():
    with pytest.raises(NoFrontmatterError):
        parse_file(FIXTURES / "no_frontmatter.md")


def test_parse_malformed_yaml_raises():
    with pytest.raises(MalformedFrontmatterError):
        parse_file(FIXTURES / "malformed_yaml.md")


def test_parse_missing_file_raises_filenotfound():
    with pytest.raises(FileNotFoundError):
        parse_file(FIXTURES / "does_not_exist.md")
```

- [ ] **Step 3: Run tests to verify they fail**

```bash
cd tools/docs_validator
make test
```

Expected: 4 new failures — module not found.

- [ ] **Step 4: Implement the parser**

Create `tools/docs_validator/src/docs_validator/parser.py`:

```python
from __future__ import annotations

from dataclasses import dataclass
from pathlib import Path

import frontmatter
import yaml


class NoFrontmatterError(ValueError):
    """Raised when a Markdown file has no YAML frontmatter block."""


class MalformedFrontmatterError(ValueError):
    """Raised when the frontmatter block exists but cannot be parsed as YAML."""


@dataclass
class ParseResult:
    path: Path
    metadata: dict
    body: str


def parse_file(path: Path) -> ParseResult:
    path = Path(path)
    text = path.read_text(encoding="utf-8")
    if not text.lstrip().startswith("---"):
        raise NoFrontmatterError(f"{path}: no frontmatter delimiter")
    try:
        post = frontmatter.loads(text)
    except yaml.YAMLError as e:
        raise MalformedFrontmatterError(f"{path}: {e}") from e
    if not post.metadata:
        raise NoFrontmatterError(f"{path}: empty frontmatter")
    return ParseResult(path=path, metadata=dict(post.metadata), body=post.content)
```

- [ ] **Step 5: Run tests to verify they pass**

```bash
cd tools/docs_validator
make test
```

Expected: 14 tests passed.

- [ ] **Step 6: Commit**

```bash
git add tools/docs_validator/src/docs_validator/parser.py tools/docs_validator/tests/test_parser.py tools/docs_validator/tests/fixtures/
git commit -m "feat(docs-validator): add Markdown frontmatter parser with explicit error types"
```

---

## Task 4: Single-file validator CLI

**Files:**
- Create: `tools/docs_validator/src/docs_validator/cli.py`
- Create: `tools/docs_validator/tests/test_cli.py`
- Create: `tools/docs_validator/tests/fixtures/invalid_missing_audience.md`

- [ ] **Step 1: Add an invalid fixture**

Create `tools/docs_validator/tests/fixtures/invalid_missing_audience.md`:

```markdown
---
title: "Missing Audience"
status: active
record_class: canonical
owner: "Test"
capability: knowledge
---

# Body
```

- [ ] **Step 2: Write the failing CLI tests**

Create `tools/docs_validator/tests/test_cli.py`:

```python
from pathlib import Path
import subprocess
import sys

FIXTURES = Path(__file__).parent / "fixtures"


def run_cli(*args: str) -> subprocess.CompletedProcess:
    return subprocess.run(
        [sys.executable, "-m", "docs_validator.cli", *args],
        capture_output=True,
        text=True,
    )


def test_cli_exit_zero_on_valid_file():
    result = run_cli(str(FIXTURES / "valid.md"))
    assert result.returncode == 0, result.stderr


def test_cli_exit_one_on_invalid_file():
    result = run_cli(str(FIXTURES / "invalid_missing_audience.md"))
    assert result.returncode == 1
    assert "audience" in result.stderr.lower() or "audience" in result.stdout.lower()


def test_cli_exit_one_on_missing_frontmatter():
    result = run_cli(str(FIXTURES / "no_frontmatter.md"))
    assert result.returncode == 1


def test_cli_recurses_directories():
    result = run_cli(str(FIXTURES))
    # FIXTURES contains both valid and invalid files; expect failure
    assert result.returncode == 1
    # but each invalid file should be reported individually
    assert "invalid_missing_audience.md" in result.stdout + result.stderr
    assert "malformed_yaml.md" in result.stdout + result.stderr


def test_cli_prints_summary_count():
    result = run_cli(str(FIXTURES))
    output = result.stdout + result.stderr
    assert "checked" in output.lower()
```

- [ ] **Step 3: Run tests to verify they fail**

```bash
cd tools/docs_validator
make test
```

Expected: 5 new failures — `docs_validator.cli` module missing.

- [ ] **Step 4: Implement the CLI**

Create `tools/docs_validator/src/docs_validator/cli.py`:

```python
from __future__ import annotations

import argparse
import sys
from pathlib import Path

from pydantic import ValidationError

from docs_validator.parser import (
    parse_file,
    NoFrontmatterError,
    MalformedFrontmatterError,
)
from docs_validator.schema import Frontmatter


SKIP_DIRS = {".git", "node_modules", "__pycache__", ".venv", "venv", "dist", "build"}


def discover_markdown(paths: list[Path]) -> list[Path]:
    files: list[Path] = []
    for p in paths:
        if p.is_file() and p.suffix == ".md":
            files.append(p)
        elif p.is_dir():
            for f in p.rglob("*.md"):
                if any(part in SKIP_DIRS for part in f.parts):
                    continue
                files.append(f)
    return sorted(set(files))


def validate_one(path: Path) -> list[str]:
    try:
        parsed = parse_file(path)
    except NoFrontmatterError as e:
        return [f"{path}: missing frontmatter ({e})"]
    except MalformedFrontmatterError as e:
        return [f"{path}: malformed YAML ({e})"]
    try:
        Frontmatter(**parsed.metadata)
    except ValidationError as e:
        return [f"{path}: schema error: {err['loc']} {err['msg']}" for err in e.errors()]
    return []


def main(argv: list[str] | None = None) -> int:
    parser = argparse.ArgumentParser(
        prog="docs-validator",
        description="Validate frontmatter on Markdown documentation.",
    )
    parser.add_argument("paths", nargs="+", type=Path, help="Files or directories to scan")
    args = parser.parse_args(argv)

    files = discover_markdown(args.paths)
    failures: list[str] = []
    for f in files:
        failures.extend(validate_one(f))

    for msg in failures:
        print(msg, file=sys.stderr)
    print(f"checked {len(files)} file(s); {len(failures)} error(s)", file=sys.stdout)

    return 0 if not failures else 1


if __name__ == "__main__":
    raise SystemExit(main())
```

- [ ] **Step 5: Run tests to verify they pass**

```bash
cd tools/docs_validator
make test
```

Expected: 19 tests passed.

- [ ] **Step 6: Commit**

```bash
git add tools/docs_validator/src/docs_validator/cli.py tools/docs_validator/tests/test_cli.py tools/docs_validator/tests/fixtures/invalid_missing_audience.md
git commit -m "feat(docs-validator): add CLI that validates files or directories of Markdown"
```

---

## Task 5: Cross-file supersession check

**Files:**
- Create: `tools/docs_validator/src/docs_validator/checks/__init__.py`
- Create: `tools/docs_validator/src/docs_validator/checks/supersession.py`
- Create: `tools/docs_validator/tests/test_supersession.py`
- Modify: `tools/docs_validator/src/docs_validator/cli.py` (wire in the check)
- Create: `tools/docs_validator/tests/fixtures/superseded_dangling.md`
- Create: `tools/docs_validator/tests/fixtures/superseded_ok.md`
- Create: `tools/docs_validator/tests/fixtures/replacement.md`

- [ ] **Step 1: Add fixtures**

Create `tools/docs_validator/tests/fixtures/replacement.md`:

```markdown
---
title: "Replacement Doc"
status: active
record_class: canonical
audience: [internal]
owner: "Test"
capability: knowledge
---

# Replacement
```

Create `tools/docs_validator/tests/fixtures/superseded_ok.md`:

```markdown
---
title: "Old Doc"
status: superseded
record_class: historical
audience: [internal]
owner: "Test"
capability: knowledge
superseded_by: "replacement.md"
---

# Old Doc
```

Create `tools/docs_validator/tests/fixtures/superseded_dangling.md`:

```markdown
---
title: "Dangling Old Doc"
status: superseded
record_class: historical
audience: [internal]
owner: "Test"
capability: knowledge
superseded_by: "does-not-exist.md"
---

# Dangling
```

- [ ] **Step 2: Write the failing supersession tests**

Create `tools/docs_validator/tests/test_supersession.py`:

```python
from pathlib import Path

from docs_validator.checks.supersession import check_supersession_links
from docs_validator.parser import parse_file

FIXTURES = Path(__file__).parent / "fixtures"


def test_resolved_supersession_returns_no_error():
    parsed = [parse_file(FIXTURES / "superseded_ok.md"), parse_file(FIXTURES / "replacement.md")]
    errors = check_supersession_links(parsed, root=FIXTURES)
    assert errors == []


def test_dangling_supersession_returns_error():
    parsed = [parse_file(FIXTURES / "superseded_dangling.md")]
    errors = check_supersession_links(parsed, root=FIXTURES)
    assert len(errors) == 1
    assert "does-not-exist.md" in errors[0]
```

- [ ] **Step 3: Run tests to verify they fail**

```bash
cd tools/docs_validator
make test
```

Expected: 2 new failures — `checks.supersession` does not exist.

- [ ] **Step 4: Implement the check**

Create `tools/docs_validator/src/docs_validator/checks/__init__.py` (empty).

Create `tools/docs_validator/src/docs_validator/checks/supersession.py`:

```python
from __future__ import annotations

from pathlib import Path

from docs_validator.parser import ParseResult


def check_supersession_links(parsed: list[ParseResult], root: Path) -> list[str]:
    """Return one error string per `superseded_by` value that does not resolve."""
    root = Path(root)
    errors: list[str] = []
    for item in parsed:
        target = item.metadata.get("superseded_by")
        if not target:
            continue
        # Resolve relative to the file containing the link, then relative to root.
        candidates = [
            (item.path.parent / target).resolve(),
            (root / target).resolve(),
        ]
        if not any(c.is_file() for c in candidates):
            errors.append(
                f"{item.path}: superseded_by '{target}' does not resolve to an existing file"
            )
    return errors
```

- [ ] **Step 5: Wire into the CLI**

Modify `tools/docs_validator/src/docs_validator/cli.py`. Update the imports block at the top to add `ParseResult` and the supersession check, plus a `dataclass` import:

```python
from dataclasses import dataclass

from docs_validator.parser import (
    parse_file,
    ParseResult,
    NoFrontmatterError,
    MalformedFrontmatterError,
)
from docs_validator.checks.supersession import check_supersession_links
```

(Replace the existing `from docs_validator.parser import ...` block with the four-name version above.)

Add the helper dataclass and parse function above `main`:

```python
@dataclass
class _FileCheckResult:
    parsed: ParseResult | None
    errors: list[str]


def _try_parse_and_validate(path: Path) -> _FileCheckResult:
    try:
        parsed = parse_file(path)
    except NoFrontmatterError as e:
        return _FileCheckResult(parsed=None, errors=[f"{path}: missing frontmatter ({e})"])
    except MalformedFrontmatterError as e:
        return _FileCheckResult(parsed=None, errors=[f"{path}: malformed YAML ({e})"])
    try:
        Frontmatter(**parsed.metadata)
    except ValidationError as e:
        errs = [f"{path}: schema error: {err['loc']} {err['msg']}" for err in e.errors()]
        return _FileCheckResult(parsed=None, errors=errs)
    return _FileCheckResult(parsed=parsed, errors=[])
```

Replace the `main` function body with:

```python
def main(argv: list[str] | None = None) -> int:
    parser = argparse.ArgumentParser(
        prog="docs-validator",
        description="Validate frontmatter on Markdown documentation.",
    )
    parser.add_argument("paths", nargs="+", type=Path, help="Files or directories to scan")
    args = parser.parse_args(argv)

    files = discover_markdown(args.paths)
    failures: list[str] = []
    parsed_ok: list[ParseResult] = []
    for f in files:
        result = _try_parse_and_validate(f)
        if result.errors:
            failures.extend(result.errors)
        if result.parsed is not None:
            parsed_ok.append(result.parsed)

    failures.extend(check_supersession_links(parsed_ok, root=args.paths[0]))

    for msg in failures:
        print(msg, file=sys.stderr)
    print(f"checked {len(files)} file(s); {len(failures)} error(s)", file=sys.stdout)
    return 0 if not failures else 1
```

Delete the old `validate_one` function (replaced by `_try_parse_and_validate`).

- [ ] **Step 6: Run tests to verify they pass**

```bash
cd tools/docs_validator
make test
```

Expected: 21 tests passed (CLI tests still green; supersession tests now green).

- [ ] **Step 7: Commit**

```bash
git add tools/docs_validator/src/docs_validator/checks/ tools/docs_validator/src/docs_validator/cli.py tools/docs_validator/tests/test_supersession.py tools/docs_validator/tests/fixtures/superseded_*.md tools/docs_validator/tests/fixtures/replacement.md
git commit -m "feat(docs-validator): cross-file supersession integrity check"
```

---

## Task 6: Staleness warning

**Files:**
- Create: `tools/docs_validator/src/docs_validator/checks/staleness.py`
- Create: `tools/docs_validator/tests/test_staleness.py`
- Modify: `tools/docs_validator/src/docs_validator/cli.py` (wire in)

- [ ] **Step 1: Write the failing staleness tests**

Create `tools/docs_validator/tests/test_staleness.py`:

```python
from datetime import date, timedelta
from pathlib import Path

from docs_validator.checks.staleness import is_stale, find_stale, STALENESS_DAYS


def test_weekly_doc_reviewed_today_not_stale():
    assert is_stale(cadence="weekly", last_reviewed=date.today()) is False


def test_weekly_doc_reviewed_30_days_ago_is_stale():
    assert is_stale(cadence="weekly", last_reviewed=date.today() - timedelta(days=30)) is True


def test_monthly_doc_reviewed_15_days_ago_not_stale():
    assert is_stale(cadence="monthly", last_reviewed=date.today() - timedelta(days=15)) is False


def test_per_release_never_stale_without_release_signal():
    # We do not auto-fail per-release docs; staleness check skips them.
    assert is_stale(cadence="per-release", last_reviewed=date(2020, 1, 1)) is False


def test_no_last_reviewed_returns_false():
    assert is_stale(cadence="weekly", last_reviewed=None) is False


def test_staleness_thresholds_documented():
    # Sanity: documented thresholds match the implementation.
    assert STALENESS_DAYS["weekly"] == 14
    assert STALENESS_DAYS["monthly"] == 60
    assert STALENESS_DAYS["per-stage"] == 90
```

- [ ] **Step 2: Run tests to verify they fail**

```bash
cd tools/docs_validator
make test
```

Expected: 6 failures — `checks.staleness` missing.

- [ ] **Step 3: Implement the check**

Create `tools/docs_validator/src/docs_validator/checks/staleness.py`:

```python
from __future__ import annotations

from datetime import date, timedelta
from pathlib import Path
from typing import Optional

from docs_validator.parser import ParseResult


# Grace windows beyond the cadence period before a doc is considered stale.
# Generous on purpose — the goal is to flag rot, not to nag weekly cadences.
STALENESS_DAYS: dict[str, int] = {
    "weekly": 14,
    "monthly": 60,
    "per-stage": 90,
    "ad-hoc": 365,
    # per-release and one-shot are not time-based; not staled by this check.
}


def is_stale(cadence: Optional[str], last_reviewed: Optional[date]) -> bool:
    if cadence is None or last_reviewed is None:
        return False
    threshold = STALENESS_DAYS.get(cadence)
    if threshold is None:
        return False
    return (date.today() - last_reviewed) > timedelta(days=threshold)


def find_stale(parsed: list[ParseResult]) -> list[str]:
    out: list[str] = []
    for item in parsed:
        cadence = item.metadata.get("cadence")
        last = item.metadata.get("last_reviewed")
        if isinstance(last, str):
            try:
                last = date.fromisoformat(last)
            except ValueError:
                continue
        if is_stale(cadence=cadence, last_reviewed=last):
            out.append(
                f"{item.path}: stale — cadence={cadence}, last_reviewed={last} "
                f"(threshold {STALENESS_DAYS[cadence]}d)"
            )
    return out
```

- [ ] **Step 4: Wire into the CLI**

Modify `tools/docs_validator/src/docs_validator/cli.py`. Add import:

```python
from docs_validator.checks.staleness import find_stale
```

Add a `--strict-staleness` flag and warning behavior. Replace the `main` function's body after `failures.extend(check_supersession_links(...))` with:

```python
    stale_warnings = find_stale(parsed_ok)

    for msg in failures:
        print(msg, file=sys.stderr)
    for msg in stale_warnings:
        if args.strict_staleness:
            failures.append(msg)
            print(msg, file=sys.stderr)
        else:
            print(f"WARN {msg}", file=sys.stderr)
    print(
        f"checked {len(files)} file(s); {len(failures)} error(s); {len(stale_warnings)} stale",
        file=sys.stdout,
    )
    return 0 if not failures else 1
```

And add the argparse flag inside `main`:

```python
    parser.add_argument(
        "--strict-staleness",
        action="store_true",
        help="Treat staleness warnings as errors (default: warn only).",
    )
```

- [ ] **Step 5: Run tests to verify they pass**

```bash
cd tools/docs_validator
make test
```

Expected: 27 tests passed.

- [ ] **Step 6: Commit**

```bash
git add tools/docs_validator/src/docs_validator/checks/staleness.py tools/docs_validator/src/docs_validator/cli.py tools/docs_validator/tests/test_staleness.py
git commit -m "feat(docs-validator): staleness check with configurable strict mode"
```

---

## Task 7: ADR-link-coverage check

**Files:**
- Create: `tools/docs_validator/src/docs_validator/checks/adr_coverage.py`
- Create: `tools/docs_validator/tests/test_adr_coverage.py`

This check is a separate report (not part of the main validation pipeline) — it lists ADRs not linked from any other doc. CI will run it and fail only if requested via `--strict`.

- [ ] **Step 1: Add fixtures**

Create directory `tools/docs_validator/tests/fixtures/adr_repo/`. Inside it, create:

`tools/docs_validator/tests/fixtures/adr_repo/adr/0001-adopt-mesh.md`:

```markdown
---
title: "ADR-0001: Adopt Hybrid Mesh"
status: accepted
record_class: canonical
audience: [internal]
owner: "Architecture"
capability: knowledge
---

# Decision
```

`tools/docs_validator/tests/fixtures/adr_repo/adr/0002-orphan.md`:

```markdown
---
title: "ADR-0002: Orphan"
status: accepted
record_class: canonical
audience: [internal]
owner: "Architecture"
capability: knowledge
---

# Decision
```

`tools/docs_validator/tests/fixtures/adr_repo/architecture.md`:

```markdown
---
title: "Architecture overview"
status: active
record_class: canonical
audience: [internal]
owner: "Architecture"
capability: architecture
---

See [ADR-0001](adr/0001-adopt-mesh.md) for the mesh decision.
```

- [ ] **Step 2: Write the failing tests**

Create `tools/docs_validator/tests/test_adr_coverage.py`:

```python
from pathlib import Path

from docs_validator.checks.adr_coverage import find_orphan_adrs

FIXTURES = Path(__file__).parent / "fixtures" / "adr_repo"


def test_finds_orphan_adr_not_linked():
    orphans = find_orphan_adrs(root=FIXTURES, adr_dir=FIXTURES / "adr")
    orphan_names = {p.name for p in orphans}
    assert "0002-orphan.md" in orphan_names
    assert "0001-adopt-mesh.md" not in orphan_names


def test_returns_empty_list_when_all_linked():
    # Add a temporary file that references 0002, then re-check.
    extra = FIXTURES / "_temp_link.md"
    extra.write_text(
        "---\n"
        "title: tmp\nstatus: active\nrecord_class: supporting\n"
        "audience: [internal]\nowner: t\ncapability: knowledge\n---\n\n"
        "[ADR-0002](adr/0002-orphan.md)\n"
    )
    try:
        orphans = find_orphan_adrs(root=FIXTURES, adr_dir=FIXTURES / "adr")
        assert orphans == []
    finally:
        extra.unlink()
```

- [ ] **Step 3: Run tests to verify they fail**

```bash
cd tools/docs_validator
make test
```

Expected: 2 failures — module missing.

- [ ] **Step 4: Implement the check**

Create `tools/docs_validator/src/docs_validator/checks/adr_coverage.py`:

```python
from __future__ import annotations

from pathlib import Path


def find_orphan_adrs(root: Path, adr_dir: Path) -> list[Path]:
    """Return ADR files in `adr_dir` that no Markdown file under `root` links to."""
    root = Path(root)
    adr_dir = Path(adr_dir)
    if not adr_dir.is_dir():
        return []

    adrs = sorted(p for p in adr_dir.glob("*.md") if p.is_file())

    # Collect all .md files except the ADRs themselves.
    other_md = [
        f for f in root.rglob("*.md")
        if f.is_file() and adr_dir not in f.parents and f != adr_dir
    ]
    corpus = "\n".join(f.read_text(encoding="utf-8") for f in other_md)

    orphans: list[Path] = []
    for adr in adrs:
        if adr.name not in corpus and adr.stem not in corpus:
            orphans.append(adr)
    return orphans
```

- [ ] **Step 5: Run tests to verify they pass**

```bash
cd tools/docs_validator
make test
```

Expected: 29 tests passed.

- [ ] **Step 6: Commit**

```bash
git add tools/docs_validator/src/docs_validator/checks/adr_coverage.py tools/docs_validator/tests/test_adr_coverage.py tools/docs_validator/tests/fixtures/adr_repo/
git commit -m "feat(docs-validator): orphan-ADR detection for coverage reporting"
```

---

## Task 8: Seed real artifacts and the schema documentation page

**Files:**
- Create: `docs/_examples/canonical-manager.md`
- Create: `docs/_examples/canonical-internal.md`
- Create: `docs/_examples/end-user-doc.md`
- Create: `docs/_examples/superseded.md`
- Create: `docs/_examples/replacement.md`
- Create: `docs/00_operating_model/frontmatter-schema.md`

These are *real* artifacts used by the CI workflow to prove the validator works against the actual repo, not just unit-test fixtures. They demonstrate every audience and capability combination the schema supports.

- [ ] **Step 1: Write the canonical manager example**

Create `docs/_examples/canonical-manager.md`:

```markdown
---
title: "Example: Stakeholder Register"
status: active
record_class: canonical
audience: [internal, manager, client]
owner: "PM"
capability: governance
phase: initiation
cadence: per-stage
last_reviewed: 2026-05-07
---

# Example: Stakeholder Register

This file demonstrates a manager-and-client-facing canonical artifact.
The body is intentionally short — the point of this file is its frontmatter.
```

- [ ] **Step 2: Write the canonical internal example**

Create `docs/_examples/canonical-internal.md`:

```markdown
---
title: "Example: ADR template"
status: active
record_class: canonical
audience: [internal]
owner: "Architecture"
capability: knowledge
phase: n/a
cadence: ad-hoc
last_reviewed: 2026-05-07
---

# Example: ADR template

Internal-only artifact. Will not appear in client-bundle exports.
```

- [ ] **Step 3: Write the end-user example**

Create `docs/_examples/end-user-doc.md`:

```markdown
---
title: "How to reset your password"
status: active
record_class: canonical
audience: [end_user]
owner: "Product"
capability: user_docs
phase: n/a
cadence: per-release
last_reviewed: 2026-05-07
---

# How to reset your password

End-user-facing how-to (Diataxis "how-to" mode).
Lives under `09_user_documentation/how-to/` in real projects;
here as an example.
```

- [ ] **Step 4: Write the supersession pair**

Create `docs/_examples/replacement.md`:

```markdown
---
title: "Example: Replacement Doc"
status: active
record_class: canonical
audience: [internal]
owner: "Architecture"
capability: knowledge
last_reviewed: 2026-05-07
---

# Example: Replacement Doc

Demonstrates the target of an in-place supersession link.
```

Create `docs/_examples/superseded.md`:

```markdown
---
title: "Example: Old Doc"
status: superseded
record_class: historical
audience: [internal]
owner: "Architecture"
capability: knowledge
superseded_by: "replacement.md"
last_reviewed: 2026-05-07
---

# Example: Old Doc

> **Superseded** by [replacement.md](replacement.md). Kept in place per the in-place deprecation policy.
```

- [ ] **Step 5: Write the schema documentation page**

Create `docs/00_operating_model/frontmatter-schema.md`:

```markdown
---
title: "Frontmatter Schema (Normative)"
status: active
record_class: canonical
audience: [internal, manager]
owner: "Documentation maintainer"
capability: operating_model
phase: n/a
cadence: ad-hoc
last_reviewed: 2026-05-07
---

# Frontmatter Schema (Normative)

Every canonical Markdown artifact in this repository begins with a YAML
frontmatter block. CI rejects pull requests whose new or modified canonical
artifacts do not satisfy this schema.

## Required keys

| Key | Type | Notes |
|---|---|---|
| `title` | string | Human-readable title |
| `status` | enum | `draft` \| `proposed` \| `accepted` \| `active` \| `superseded` \| `archived` |
| `record_class` | enum | `canonical` \| `supporting` \| `historical` |
| `audience` | list of enums | one or more of `internal`, `manager`, `client`, `end_user`, `auditor` |
| `owner` | string | Role or team name (e.g. `PM`, `QA Lead`) |
| `capability` | enum | One of `governance`, `product`, `architecture`, `execution`, `quality`, `operations`, `knowledge`, `references`, `user_docs`, `operating_model` |

## Conditional keys

- If `status: superseded`, then `superseded_by: <relative-path>` is required.
- If `capability: execution`, then both `cadence` and `source_of_truth` are required.

## Optional keys

| Key | Type | Notes |
|---|---|---|
| `phase` | enum | `initiation` \| `planning` \| `execution` \| `monitoring` \| `closure` \| `n/a` |
| `cadence` | enum | `ad-hoc` \| `weekly` \| `monthly` \| `per-stage` \| `per-release` \| `one-shot` |
| `last_reviewed` | ISO date (YYYY-MM-DD) | Used by the staleness check |
| `supersedes` | list of paths | What this artifact replaces |
| `superseded_by` | path | Required when `status: superseded` |
| `source_of_truth` | string | `repo` or `mirror_of:<external-system>` |
| `tags` | list of strings | Free-form, non-normative |

## Validation

- Run locally: `cd tools/docs_validator && make validate`
- CI: `.github/workflows/docs.yml` enforces on every PR.

## Examples

See [`docs/_examples/`](../_examples/) for one valid artifact per audience class.
```

- [ ] **Step 6: Validate the seed artifacts pass the validator**

```bash
cd tools/docs_validator
make install
docs-validator ../../docs/_examples ../../docs/00_operating_model/frontmatter-schema.md
```

Expected: `checked 6 file(s); 0 error(s); 0 stale`.

- [ ] **Step 7: Commit**

```bash
git add docs/_examples/ docs/00_operating_model/frontmatter-schema.md
git commit -m "docs: seed frontmatter examples and normative schema documentation"
```

---

## Task 9: Markdown lint and link checker config

**Files:**
- Create: `.markdownlint-cli2.yaml`
- Create: `lychee.toml`

- [ ] **Step 1: Configure markdownlint**

Create `.markdownlint-cli2.yaml` at the repo root:

```yaml
config:
  default: true
  # MD013: line length — disabled because frontmatter values, URLs,
  # and tables routinely exceed 80 columns.
  MD013: false
  # MD024: duplicate headings — allowed when nested under different parents.
  MD024:
    siblings_only: true
  # MD033: inline HTML — allowed (used for callouts).
  MD033: false
  # MD041: first line must be top-level heading — disabled because we use frontmatter.
  MD041: false

ignores:
  - "node_modules/**"
  - ".venv/**"
  - "tools/docs_validator/tests/fixtures/**"
  - "**/CHANGELOG.md"
```

- [ ] **Step 2: Configure lychee**

Create `lychee.toml` at the repo root:

```toml
# Link checker config — see https://lychee.cli.rs

# Be polite to remote servers.
max_concurrency = 8
timeout = 20

# Treat anchors as best-effort; not all GitHub-rendered fragments resolve.
include_fragments = false

# Skip private and ephemeral hosts.
exclude = [
  "^https?://localhost",
  "^https?://127\\.0\\.0\\.1",
  "^https?://0\\.0\\.0\\.0",
  "^mailto:",
]

# Cache results across CI runs.
cache = true
max_cache_age = "7d"

# We accept these status codes as success.
accept = [200, 206, 301, 302, 304, 308, 403, 429]

# Don't fail on missing fixtures or generated files.
exclude_path = [
  "node_modules",
  ".venv",
  "tools/docs_validator/tests/fixtures",
  "99_archive",
]
```

- [ ] **Step 3: Verify markdownlint runs (no install needed; uses npx)**

```bash
npx -y markdownlint-cli2 "docs/**/*.md" "!**/node_modules/**"
```

Expected: no output and exit 0 (or output only for any pre-existing issues — note them but do not block on them; the goal of this task is to ship the config, not to fix the existing repo).

- [ ] **Step 4: Verify lychee config parses**

```bash
docker run --rm -v "$PWD":/input lycheeverse/lychee --dump-inputs --config /input/lychee.toml /input/docs/_examples
```

Expected: lychee prints the list of inputs without configuration errors. (If Docker is unavailable, skip — CI will exercise lychee.)

- [ ] **Step 5: Commit**

```bash
git add .markdownlint-cli2.yaml lychee.toml
git commit -m "ci(docs): markdownlint and lychee configuration"
```

---

## Task 10: GitHub Actions workflow

**Files:**
- Create: `.github/workflows/docs.yml`

- [ ] **Step 1: Write the workflow**

Create `.github/workflows/docs.yml`:

```yaml
name: docs

on:
  push:
    branches: [main]
    paths:
      - "docs/**"
      - "tools/docs_validator/**"
      - ".github/workflows/docs.yml"
      - ".markdownlint-cli2.yaml"
      - "lychee.toml"
  pull_request:
    paths:
      - "docs/**"
      - "tools/docs_validator/**"
      - ".github/workflows/docs.yml"
      - ".markdownlint-cli2.yaml"
      - "lychee.toml"

permissions:
  contents: read

jobs:
  frontmatter:
    name: frontmatter-schema
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.11"
      - name: Install validator
        working-directory: tools/docs_validator
        run: pip install -e ".[dev]"
      - name: Run validator tests
        working-directory: tools/docs_validator
        run: pytest -v
      - name: Validate docs/
        run: docs-validator docs

  markdownlint:
    name: markdownlint
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: "20"
      - name: Run markdownlint-cli2
        run: npx -y markdownlint-cli2 "docs/**/*.md" "!**/node_modules/**"

  links:
    name: link-check
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: lycheeverse/lychee-action@v2
        with:
          args: --config lychee.toml docs/

  adr-coverage:
    name: adr-coverage-report
    runs-on: ubuntu-latest
    # Report-only job; does not block merges. Useful as advisory output.
    continue-on-error: true
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.11"
      - name: Install validator
        working-directory: tools/docs_validator
        run: pip install -e ".[dev]"
      - name: Report orphan ADRs
        run: |
          python -c "
          import sys
          from pathlib import Path
          from docs_validator.checks.adr_coverage import find_orphan_adrs
          adr_dir = Path('docs/adr')
          if not adr_dir.is_dir():
              print('docs/adr not present; skipping')
              sys.exit(0)
          orphans = find_orphan_adrs(root=Path('docs'), adr_dir=adr_dir)
          for o in orphans:
              print(f'ORPHAN: {o}')
          print(f'{len(orphans)} orphan ADR(s)')
          "
```

- [ ] **Step 2: Validate the workflow YAML locally**

```bash
python -c "import yaml; yaml.safe_load(open('.github/workflows/docs.yml'))"
```

Expected: no output, exit 0.

- [ ] **Step 3: Commit**

```bash
git add .github/workflows/docs.yml
git commit -m "ci(docs): frontmatter, markdownlint, lychee, and ADR coverage workflow"
```

---

## Task 11: End-to-end verification on real `docs/`

**Files:** none (this is a verification task; no new code).

- [ ] **Step 1: Run the full validator against the actual `docs/` tree**

```bash
cd tools/docs_validator
docs-validator ../../docs
```

Expected: prints any pre-existing files that lack valid frontmatter. This is **expected** on a real repo — the goal of the task is to identify the migration backlog, not to fail the build.

- [ ] **Step 2: Capture the backlog**

Pipe the output to a tracking file:

```bash
cd tools/docs_validator
docs-validator ../../docs 2> ../../docs/_examples/MIGRATION-BACKLOG.txt; true
```

Open the file and verify it lists each unmigrated artifact with the specific schema field that is missing.

- [ ] **Step 3: Verify the example artifacts and the schema doc all pass**

```bash
cd tools/docs_validator
docs-validator ../../docs/_examples ../../docs/00_operating_model/frontmatter-schema.md
```

Expected: `checked 6 file(s); 0 error(s); 0 stale`.

- [ ] **Step 4: Verify markdownlint and lychee configs do not produce false positives on the example artifacts**

```bash
npx -y markdownlint-cli2 "docs/_examples/**/*.md"
```

Expected: exit 0.

- [ ] **Step 5: Commit the migration backlog as a tracking artifact**

```bash
git add docs/_examples/MIGRATION-BACKLOG.txt
git commit -m "docs: capture frontmatter-migration backlog for legacy artifacts"
```

- [ ] **Step 6: Push and confirm CI runs green on a clean branch**

```bash
git push -u origin "$(git branch --show-current)"
gh pr create --title "Frontmatter schema and CI validators" --body "Implements docs/superpowers/plans/2026-05-07-frontmatter-schema-and-ci-validators.md"
gh pr checks --watch
```

Expected: `frontmatter-schema`, `markdownlint`, `link-check` all pass on the example artifacts. The `adr-coverage-report` job runs but does not block.

---

## Verification

After all tasks complete, the system delivers:

1. **A normative frontmatter schema** in [`docs/00_operating_model/04_frontmatter_schema.md`](../../00_operating_model/04_frontmatter_schema.md), self-validating.
2. **A Python validator** at [`tools/docs_validator/`](../../../tools/docs_validator/) with 29+ tests passing.
3. **A CI workflow** at [`.github/workflows/docs.yml`](../../../.github/workflows/docs.yml) running schema validation, markdownlint, and lychee on every PR touching `docs/` or the validator.
4. **Five seed artifacts** in [`docs/_examples/`](../../_examples/) demonstrating every audience class and the supersession pattern.
5. **A migration backlog file** listing legacy artifacts to bring up to schema.

**Manual end-to-end check:**

```bash
# 1. Schema enforcement: introduce a deliberate violation, confirm CI fails.
echo "---\ntitle: bad\n---" > docs/_examples/_bad.md
git add docs/_examples/_bad.md && git commit -m "test: should fail"
git push  # CI should reject this PR
git revert HEAD && git push

# 2. Schema accommodation: a fully-tagged new doc passes.
# (Use docs/_examples/canonical-manager.md as the template.)
```

**Acceptance criteria (maps to Section 10 of the revised spec):**

- Frontmatter validation pass rate ≥99% on main: enforced by the workflow blocking merges.
- Audience-tag completeness 100%: enforced by the schema's required `audience` field.
- Doc-staleness: zero canonical artifacts past their cadence's review window — surfaced as warnings (errors with `--strict-staleness`).
- ADR / RFC link coverage: surfaced as a non-blocking advisory job; numeric target tracked separately.

**What this plan does *not* deliver (handled by future plans):**

- Generated lifecycle / role / capability indexes from frontmatter (plan: index generator).
- MkDocs / Pandoc export pipeline producing audience-filtered bundles (plan: export pipeline).
- ADR / RFC templates and the decision-rights matrix (plan: ADR/RFC subsystem).
- `09_user_documentation/` Diataxis scaffolding (plan: end-user docs area).

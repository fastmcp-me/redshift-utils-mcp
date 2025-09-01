# CI Fix Workflow

When CI fails, follow these steps:

## 1. Check Latest Workflow Run
```
mcp__github__list_workflow_runs
- owner: vinodismyname
- repo: redshift-utils-mcp
- workflow_id: 185744589  # CI Tests workflow
- perPage: 1
```

## 2. Get Failed Job Logs
```
mcp__github__get_job_logs
- owner: vinodismyname
- repo: redshift-utils-mcp
- run_id: [from step 1]
- failed_only: true
- return_content: true
- tail_lines: 200
```

## 3. Fix Issues Locally
Based on the error type:
- **Black formatting**: Run `uv run black src/`
- **Ruff linting**: Run `uv run ruff check src/ --fix`
- **Test failures**: Run `uv run pytest tests/ -v` to reproduce

## 4. Verify Fixes
```bash
uv run black --check src/
uv run ruff check src/
uv run pytest tests/
```

## 5. Commit and Push
```bash
git add -A
git commit -m "fix: [description of what was fixed]"
git push origin main
```

## 6. Monitor New Run
Wait ~30 seconds, then repeat step 1 to check if the new run passes.
If it fails, go back to step 2.
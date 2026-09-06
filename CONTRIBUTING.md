# Contributing

## Repository Structure

This repository provides reusable GitHub Actions workflows for the
`eingram-homelab` organization.

- Files beginning with `_` in `.github/workflows/` are reusable workflows.
- Other files in `.github/workflows/` are workflows used by this repository.
- Flattened workflows combine reusable workflows for a specific project type.

Caller repositories should use the appropriate flattened workflow and pin it
to the `main` branch. For example:

```yaml
uses: eingram-homelab/reusable-workflows/.github/workflows/_flattened-terraform-module-pr-workflow.yaml@main
```

## Repository Workflows

The repository uses these entrypoint workflows:

- `f-branch-validate.yaml` validates pushes to non-`main` branches.
- `pr-workflow.yaml` validates pull requests targeting `main`.
- `release-reusable-workflows.yaml` tags changed reusable workflows.
- `release-actions.yaml` tags changed actions.

Validation is advisory for feature branches and merge-blocking for pull
requests. Changes to `main` are made through pull requests.

## Testing Changes

### Feature Branch Validation

1. Create a feature branch in this repository.
2. Make the change and push the branch.
3. Confirm that `f-branch-validate.yaml` completes successfully.

When testing a caller repository against an unreleased branch of this
repository, point its `uses` reference at the branch under test. Restore the
reference to `@main` before opening the final pull request.

### Pull Request Validation

1. Create a branch from `main` for the change.
2. Push the branch and open a pull request targeting `main`.
3. Confirm that `pr-workflow.yaml` completes successfully.

For changes that must be tested from a caller repository, use a temporary
branch in this repository and reference that branch from the caller. Restore
all references to `@main` before merging.

## Release Versioning

Release workflows run on pushes to `main` and calculate semantic version
bumps from commit messages:

- `BREAKING CHANGE:` or a `!:` commit type produces a major bump.
- `feat:` or `feat(...):` produces a minor bump.
- All other changes produce a patch bump.

Reusable workflow tags use the workflow filename without its leading `_` and
file extension. For example, `_validate-workflow.yaml` produces tags such as
`validate-workflow-v1.2.3`.

Action tags use the action directory name, such as
`actions-semver-tag-v1.2.3`.
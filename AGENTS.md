# XDenovo Organization GitHub Defaults

## Repository Role

- This is the public `XDenovo/.github` repository. It owns organization-default collaboration files and the public organization profile.
- It may also host explicitly reusable workflows and organization workflow templates.
- It does not own application CI, repository-specific contribution instructions, or Platform-wide GitHub policy. The policy source is [XDenovo Platform GitHub conventions](https://github.com/XDenovo/platform/blob/main/docs/github.md).
- Keep this repository public; most organization-default community health files are not inherited from a private `.github` repository.

## Path Ownership

| Path | Owns |
|---|---|
| `.github/ISSUE_TEMPLATE/` | Default Issue Forms and template chooser configuration |
| `.github/PULL_REQUEST_TEMPLATE.md` | Default Pull Request body |
| `profile/README.md` | Public XDenovo organization profile |
| Root community health files | Organization-default contribution, security, support, governance, and conduct guidance |
| `.github/workflows/` | Workflows that run in this repository, plus workflows explicitly exposed through `workflow_call` |
| `workflow-templates/` | Optional workflows users can choose when creating a workflow in another XDenovo repository |

## Default Inheritance and Overrides

- Organization defaults apply only when a target repository does not provide its own file of that type.
- Treat `.github/ISSUE_TEMPLATE/` as one override unit: if a target repository has any local Issue template or `config.yml`, GitHub does not supplement it with templates from this repository.
- Respect intentional repository-local overrides. Change the organization default only when the desired behavior is broadly applicable.
- Keep default files on this repository's default branch; other branches do not affect the organization defaults.
- A normal file in `.github/workflows/` runs only for events in this repository. It is not copied to or automatically enabled in other repositories.
- A reusable workflow must declare `workflow_call` and be invoked explicitly by each caller repository.
- A workflow template belongs in `workflow-templates/` with a same-basename `.properties.json`. Choosing a template creates a repository-local workflow; later template changes do not update that copy.

## Issue Forms

- Use organization Issue Types as the primary work classification:

| Template | Issue Type | Purpose |
|---|---|---|
| Feature Spec | `Feature` | New capability or observable behavior |
| Bug Report | `Bug` | Existing behavior that does not meet expectations |
| Chore | `Task` | Refactoring, dependencies, tooling, or maintenance |
| Initiative | `Initiative` | A goal that requires coordinated work across repositories |

- Do not duplicate Issue Types with synonymous default labels or title prefixes. Use labels only for cross-cutting information such as domain, priority, blocked state, or a decision requirement.
- A default label is useful only when it already exists in every repository that inherits the template. Prefer no default label over a label that fails inconsistently.
- Keep form IDs stable because automation and downstream tooling may depend on them.
- Require enough information for the Issue to be implemented and verified independently. Mark unresolved decisions explicitly instead of prompting authors to guess.
- Feature Specs own repository-local requirements and measurable success criteria. Bug Reports own observed behavior, expected behavior, reproduction, and environment. Chores own the change and its closeable result.
- Initiatives own the cross-repository goal, involved repositories, dependencies, sequencing, success criteria, and explicit exclusions. Per-repository implementation details belong in sub-issues.
- Keep the Initiative repository choices aligned with the managed repository map in `XDenovo/platform`. Include managed repositories such as `dot-github`; exclude reference-only, deprecated, and otherwise excluded repositories.
- Keep blank Issues disabled unless an approved support or discussion route requires them.

## Pull Request Template

- Treat the linked Issue as the implementation specification and the Pull Request as the default result report. For a permitted direct delivery, the commit plus the Issue completion record is the result report.
- Ask for the reason for the change, concrete modifications, evidence from checks actually run, acceptance-criteria status, breaking changes, and the closing Issue reference.
- Do not ask authors to duplicate the full specification or claim checks that were not run.
- Keep closing-keyword guidance scoped to an Issue in the same repository; a repository PR must not close a parent Platform Initiative.

## Organization Profile and Community Files

- Treat `profile/README.md` and all inherited community files as public-facing content.
- Describe only public, current, and verifiable products, repositories, links, and collaboration channels.
- Do not expose private endpoints, credentials, non-public infrastructure details, customer data, or unreleased product claims.
- Before adding or retaining a featured repository, confirm that it is still active, intentionally public, and consistent with current XDenovo direction.
- Keep organization defaults broadly reusable. Repository-specific build, test, release, deployment, and vulnerability-response details belong in the owning repository.
- Use canonical GitHub or public website URLs rather than workspace-relative links.

## GitHub Actions and Automation

- Distinguish a repository workflow, a reusable workflow, and a workflow template before choosing a path. They have different execution and update behavior.
- Keep default `GITHUB_TOKEN` permissions empty or read-only and grant only the permissions required by each job.
- Pin external Actions to full commit SHAs and retain a comment identifying the corresponding release.
- Give every job a bounded timeout.
- Use `pull_request_target` only for trusted metadata operations. Never check out, download, build, parse, or execute pull-request-controlled content in that context.
- Do not interpolate untrusted event fields directly into shell code. Pass validated values through environment variables or structured tool inputs.
- Use short-lived, least-privilege GitHub App installation tokens for organization automation. Do not introduce personal access tokens as long-lived shared credentials.
- Reusable workflows must declare required inputs and secrets explicitly. Do not use broad `secrets: inherit` without a documented need and review of every inherited secret.
- Reference reusable workflows from callers with an immutable commit SHA or an approved immutable release tag.
- Project metadata automation may fail independently of application CI. Do not make Project insertion a required application check or merge blocker.
- A metadata-only workflow that handles fork Pull Requests must remain metadata-only for its entire lifetime.

## Development and Validation

- Derive checks from committed tooling. Do not invent package-manager, lint, or schema-validation commands that have not been configured.
- For Markdown and configuration-only changes, review affected links and run `git diff --check`.
- Review Issue Forms for valid YAML, supported top-level keys, unique and stable IDs, required-field behavior, and exact organization Issue Type names.
- Review workflows for trigger safety, permission scope, secret flow, expression use, immutable Action references, and timeouts.
- Do not add placeholder CI that succeeds without validating a real template, form, profile, or workflow artifact.
- `TODO: When Issue Form validation tooling is configured, pin it and document the exact local and CI commands.`
- `TODO: When actionlint or equivalent workflow validation is configured, pin it and document the exact local and CI commands.`
- `TODO: When the first reusable workflow lands, add a minimal caller fixture that exercises its inputs, permissions, secrets, and failure behavior.`
- `TODO: When the first organization workflow template lands, validate its YAML and matching properties.json together.`
- `TODO: When organization profile or community-health content expands, configure and document link validation.`

## Git and GitHub Workflow

- Update the owning Platform policy document before changing organization-wide Issue semantics, Project workflow, automation identity, merge policy, or release conventions.
- A template wording or implementation change that follows existing policy may use a self-contained Issue in this repository.
- Preserve unrelated work, stage only intended paths, and report checks actually run.
- Follow Conventional Commits.
- This repository permits direct delivery on `main`: a maintainer may work from a clean local `main` synchronized with `origin/main`, validate the intended changes, stage only explicit paths, commit, and push normally to `origin/main`. Never force-push.
- A Pull Request is optional. Prefer one when independent review is useful or the change affects workflow permissions, secret flow, automation identity, or another broad organization boundary.
- When direct delivery completes an Issue, record the commit and checks in the Issue and close it manually after the push.

## Canonical References

| Topic | Source |
|---|---|
| XDenovo GitHub collaboration policy | [XDenovo Platform GitHub conventions](https://github.com/XDenovo/platform/blob/main/docs/github.md) |
| Default file inheritance | [Creating a default community health file](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file) |
| Issue Form schema | [Syntax for Issue Forms](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/syntax-for-issue-forms) |
| Reusable workflows | [Reusing workflows](https://docs.github.com/en/actions/how-tos/reuse-automations/reuse-workflows) |
| Organization workflow templates | [Creating workflow templates for your organization](https://docs.github.com/en/actions/how-tos/reuse-automations/create-workflow-templates) |

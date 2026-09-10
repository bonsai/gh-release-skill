# GitHub Release Skill

## Role

You are responsible for deciding whether a change should become a GitHub Release and, when explicitly intended, executing the release through GitHub's existing REST API.

## Inputs

Determine:

- target repository
- current/default branch
- latest release and tag
- requested or inferred version
- release intent
- stable / prerelease
- draft / published
- release notes source

## Decision policy

Release only when the change produces an intentional, versioned, user-consumable artifact or product.

### Releaseable

- browser extension/package
- CLI or executable
- library/package
- deployable application
- versioned dataset snapshot
- packaged template or other distributable artifact

### Not releaseable by default

- source-only commit
- docs-only change
- Issue or PR
- CI/workflow-only change
- test-only change
- internal configuration
- unfinished/draft work

A tag alone is not sufficient evidence of release intent.

## Version policy

Use `vMAJOR.MINOR.PATCH` for stable releases. A prerelease may use a suffix such as `-alpha.1`, `-beta.1`, or `-rc.1`.

If the user explicitly supplies a version, preserve it when valid. Otherwise inspect existing tags/releases and project conventions before choosing the next version. Do not silently invent a breaking-version bump.

## Preflight

Before creating a release:

1. Confirm the target repository.
2. Confirm the release intent.
3. Confirm the target commit/ref.
4. Confirm the version/tag is valid and not unintentionally reused.
5. Confirm the artifact is actually distributable.
6. Check CI/status when available.
7. Determine release notes.
8. Prefer draft when the user asks for review before publication.

## Execution

Use GitHub's existing Releases REST API. Do not create a custom REST server merely to wrap GitHub.

Conceptual request:

```http
POST /repos/{owner}/{repo}/releases
```

with the selected tag, release name/body, and draft/prerelease flags.

If direct REST execution is unavailable in the current agent environment, use the repository's configured GitHub Actions release workflow when one exists, or provide the exact `gh api` command needed. Never claim that a release was created without verification.

## Verification

After execution, verify that GitHub reports the release and that its tag points to the intended ref. Return:

- repository
- tag
- release name
- published/draft state
- prerelease state
- release URL

## Safety

Never publish merely because a tag exists. Never convert an ordinary commit into a public release without release intent. If the intended version or release status is ambiguous, prefer a draft or ask for clarification rather than guessing.

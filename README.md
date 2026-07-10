# GitHub Actions impostor-commit canary

This repository is a harmless security demonstration.

It exists to show why full SHA pinning in GitHub Actions is necessary but insufficient. A commit SHA can be immutable while still not belonging to the repository shown in a workflow `uses:` reference.

## What this demonstrates

A workflow can appear to reference a trusted action:

```yaml
uses: actions/checkout@[full-sha]
```

while the pinned SHA is not reachable from the declared actions/checkout repository.

That is the impostor-commit pattern.

The lesson is:

SHA pinning gives immutability. It does not, by itself, prove repository provenance.

## Safety statement

This repository contains:

* no malware
* no credential access
* no network callbacks
* no telemetry
* no persistence
* no production-use action

# **DO NOT USE THIS ACTION IN PRODUCTION.**

It exists only to demonstrate the impostor-commit pattern and the value of deterministic provenance checks such as Zizmor.

### Expected demo

A separate demo repository should contain two workflows:

| Workflow | Purpose | Expected result |
|-------|-------|-------|
| A — ungated | Shows that an impostor commit can execute | Canary warning appears in logs |
| B — provenance-gated | Runs a provenance check before the protected job | Zizmor detects impostor-commit and fails closed | 

## Defensive controls

If you take GitHub Actions security seriously, treat workflow actions as executable dependencies.

### Recommended controls:

* require full SHA pinning for third-party actions
* verify that each SHA is reachable from the declared owner/repo
* run Zizmor for impostor-commit and related workflow risks
* use deterministic tooling such as pinact for action pinning
* use lockfile-style tools such as ghasum or gh-actions-lockfile for integrity evidence and transitive visibility
* prefer curated internal actions and reusable workflows
* use self-hosted runners with approved tooling preinstalled
* avoid privileged workflows that bootstrap tooling using curl, wget, npx, or uvx

## Key point

Treat GitHub Actions like code dependencies — because they are.

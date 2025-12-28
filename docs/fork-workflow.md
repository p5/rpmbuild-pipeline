# Fork Workflow

This repository is a patched fork of [konflux-ci/rpmbuild-pipeline](https://github.com/konflux-ci/rpmbuild-pipeline). The `main` branch contains patches on top of upstream that are pending contribution back.

## Repository Structure

```
upstream/main:  A---B---C          (konflux-ci/rpmbuild-pipeline)
                        \
fork/main:               C---P1---P2---P3  (patches on top)
```

Each patch on `main` corresponds to either:
- An open PR against upstream
- A change pending PR submission

## Setup

Add the upstream remote (one-time):

```bash
git remote add upstream https://github.com/konflux-ci/rpmbuild-pipeline.git
git fetch upstream
```

## Current Patches

| Commit | Description | Upstream PR |
|--------|-------------|-------------|
| b3d19fd | fix placement of params.build-platforms | [#128](https://github.com/konflux-ci/rpmbuild-pipeline/pull/128) |
| 56bc13c | import-to-quay: silence "no such file" warnings from tar | [#131](https://github.com/konflux-ci/rpmbuild-pipeline/pull/131) |
| 7782445 | import-to-quay: more strict code extraction | [#131](https://github.com/konflux-ci/rpmbuild-pipeline/pull/131) |
| d4634d8 | Normalize NVR for OCI tag compatibility in import-to-quay | [#134](https://github.com/konflux-ci/rpmbuild-pipeline/pull/134) |

## Updating from Upstream

When upstream has new commits, rebase your patches on top:

```bash
git fetch upstream
git checkout main
git rebase upstream/main
git push origin main --force
```

Resolve any conflicts that arise during rebase.

## When a PR is Merged Upstream

When one of your PRs is merged into upstream, the corresponding patch(es) should be dropped:

```bash
git fetch upstream
git checkout main
git rebase upstream/main
# Git will pause on commits that are now redundant
git rebase --skip  # for each already-merged patch
git push origin main --force
```

Update the "Current Patches" table in this document to remove the merged entry.

## Contributing a New Patch

1. **Create the patch on a feature branch from upstream/main:**

   ```bash
   git checkout -b feature/my-change upstream/main
   # make changes
   git commit -m "Description of change"
   ```

2. **Open a PR upstream:**

   ```bash
   git push origin feature/my-change
   gh pr create --repo konflux-ci/rpmbuild-pipeline
   ```

3. **Add the patch to your fork's main:**

   ```bash
   git checkout main
   git cherry-pick <commit-sha>
   git push origin main --force
   ```

4. **Update this document** with the new patch in the "Current Patches" table.

## Adding an Existing Upstream PR

To include a patch from an upstream PR that you didn't author:

```bash
# Fetch the PR
git fetch upstream pull/<PR-NUMBER>/head:pr/<PR-NUMBER>

# Cherry-pick onto main
git checkout main
git cherry-pick <commit-sha>  # use commits from the pr/<PR-NUMBER> branch
git push origin main --force
```

## Best Practices

- **Keep patches atomic**: One logical change per commit
- **Maintain independence**: Avoid patches that depend on other uncommitted patches when possible
- **Tag before rebasing**: `git tag backup-$(date +%Y%m%d)` provides a recovery point
- **Update documentation**: Keep the patches table current
- **Force-push is expected**: The fork's `main` is a moving target by design

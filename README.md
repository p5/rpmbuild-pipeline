# Tekton Pipeline for Building RPMs (Hummingbird Fork)

This is the **Hummingbird fork** of the Tekton RPM Build Pipeline, based on the
[upstream Konflux rpmbuild-pipeline][upstream].  It adds **Pulp side tag
support**, allowing built RPMs to be uploaded into a specific Pulp RPM
repository (side tag) for coordinated multi-package updates.

You can either:

- **Copy-paste** the pipeline code into your own project (if you need to
  customize it), or
- **Reference** it using the [Git resolver][] (preferred if the predefined logic
  and [parameters](docs/parameters.md) work for you).

## Side Tag Support

Pass the `side-tag` parameter with the name of an existing Pulp RPM repository.
Built RPMs will be uploaded into that repository in addition to the standard
Quay/OCI upload.  Multiple pipeline runs can target the same side tag for
coordinated multi-package updates.

See [parameters](docs/parameters.md) and the
[example PipelineRun](.tekton/example-side-tag-build.yaml) for usage.


## How It Works

In short, based on the provided [parameters](docs/parameters.md), the pipeline
retrieves RPM package source code from a [DistGit][]-like repository and builds
RPMs using [Mock][].  The pipeline allocates architecture-specific workers to
run *Mock* builds in the appropriate environment.

For more technical details, see the [architecture
overview](docs/architecture.md).

**About to [start building RPMs in Konflux](docs/onboarding.md)?  Happy
building!**

## Help the project

We'd love your feedback and contributions.  Please open issues, or
[contribute](CONTRIBUTING.md)!

This project is sponsored by [Red Hat](https://www.redhat.com/).
[Buy](https://www.redhat.com/en/store) Red Hat subscription to sponsor this
project.

[upstream]: https://github.com/konflux-ci/rpmbuild-pipeline
[git resolver]: https://tekton.dev/docs/pipelines/git-resolver/#pipeline-resolution
[Fedora flavor]: https://gitlab.com/fedora/infrastructure/konflux/rpmbuild-pipeline
[Konflux]: https://konflux-ci.dev/docs/getting-started/
[DistGit]: https://github.com/release-engineering/dist-git
[Mock]: https://rpm-software-management.github.io/mock/
[mock-image]: https://github.com/konflux-ci/rpmbuild-pipeline-environment-container
[issues]: https://github.com/hummingbird-org/rpmbuild-pipeline/issues

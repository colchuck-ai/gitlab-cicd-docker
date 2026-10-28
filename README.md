# `gitlab-cicd-docker`

GitLab CI/CD components related to Docker workflows.

## Components

### `.generate-semver-tags`

#### Description

The `.generate-semver-tags` script is a Bash-based implementation designed to parse a semantic version (SemVer) string and generate an array of meaningful tags. These tags are commonly used in Docker image versioning and GitLab CI/CD workflows.

##### Functionality

- Extracts **major, minor, patch, pre-release, and build metadata** components from a given SemVer string, provided through the `SEMVER_STRING` environment variable.
- Generates an array of version tags following the [Semantic Versioning](https://semver.org/) standard, outputs in the `SEMVER_TAGS` array environment variable.
- Supports different types of SemVer strings, including:
  - Standard releases (e.g., `1.2.3`)
  - Pre-releases (e.g., `1.2.3-beta.1`)
  - Build metadata (e.g., `1.2.3+build.45`)
  - Combined pre-release and build metadata (e.g., `1.2.3-beta.1+build.45`)
- Ensures the highest-level version is tagged as `latest` when applicable.

#### Tagging Strategy

| `SEMVER_STRING`         | `SEMVER_TAGS[*]`                                                                    |
| ----------------------- | ----------------------------------------------------------------------------------- |
| `1.2.3`                 | `1.2.3`, `1.2`, `1`, `latest`                                                       |
| `1.2.3-beta.1`          | `1.2.3-beta.1`, `1.2-beta.1`, `1-beta.1`, `beta.1`, `beta`                          |
| `1.2.3+build.45`        | `1.2.3+build.45`                                                                    |
| `1.2.3-beta.1+build.45` | `1.2.3-beta.1`, `1.2-beta.1`, `1-beta.1`, `beta.1`, `beta`, `1.2.3-beta.1+build.45` |

#### Usage

```
include:
  - component: gitlab.example.com/engineering/dev-tools/gitlab-cicd-components/gitlab-cicd-docker/generate-semver-tags

generate-semver-tags:
  script:
    - SEMVER_STRING=$CI_COMMIT_TAG
    - !reference [ .generate-semver-tags, script ]
    - echo "Generated semvar tags: ${SEMVER_TAGS[*]}"
```

## Disclaimer

We built this because we needed it. We welcome contributions. If you want to consume the artifacts released by this project, do so with the same consideration you would take when consuming an open source component.

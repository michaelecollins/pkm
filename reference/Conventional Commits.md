---
date: July 11, 2024
epoch: 1720706221
aliases: #synonyms
tags:
---

# Conventional Commits
A specification for standardizing commit messages. It provides an easy set of rules for creating an explicit commit history.

## Table of Contents
- [Conventional Commits](#conventional-commits)
  - [Table of Contents](#table-of-contents)
  - [Structure](#structure)
  - [Commit Types](#commit-types)
  - [Breaking Changes](#breaking-changes)
  - [Examples](#examples)
    - [Commit message with description and breaking change footer](#commit-message-with-description-and-breaking-change-footer)
    - [Commit message with `scope` and `!` to draw attention to breaking change](#commit-message-with-scope-and--to-draw-attention-to-breaking-change)
    - [Commit message with multi-paragraph body and multiple footers](#commit-message-with-multi-paragraph-body-and-multiple-footers)
  - [References](#references)

## Structure
With conventional conventions, commit messages are structured as follows:
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Commit Types
| **type** | **description**                                                                                                                                           | **emoji** |
| -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| build    | Related specifically to changes that affect the build system or external dependencies.                                                                    | 🛠        |
| chore    | Denotes changes to the build process or auxiliary tools and libraries such as documentation generation, project build, or package manager configurations. | ♻️        |
| ci       | Stands for changes to CI configuration files and scripts.                                                                                                 | ⚙️        |
| docs     | Signifies changes to the documentation only.                                                                                                              | 📚        |
| feat     | Indicates a new feature for the user.                                                                                                                     | ✨         |
| fix      | Used when fixing a bug for the user.                                                                                                                      | 🐛        |
| perf     | Used for changes that improve performance.                                                                                                                | 🚀        |
| refactor | Indicates a code change that neither fixes a bug nor adds a feature.                                                                                      | 📦        |
| style    | Refers to changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc.).                                        | 💎        |
| test     | Applies to commits that add missing tests or correct existing tests.                                                                                      | 🚨        |

## Breaking Changes
Breaking changes are declared by including `BREAKING CHANGE: <message>` in the footer of the commit message. Optionally, you can signify a breaking change by including a `!` after the commit type.

## Examples
### Commit message with description and breaking change footer
```
feat: allow provided config object to extend other configs

BREAKING CHANGE: `extends` key in config file is now used for extending other config files
```

### Commit message with `scope` and `!` to draw attention to breaking change

```
chore(web)!: drop support for Node 6

BREAKING CHANGE: use JavaScript feature not available in Node 6.
```

### Commit message with multi-paragraph body and multiple footers
```
fix: prevent racing of requests

Introduce a request id and a reference to latest request. Dismiss incoming responses other than from latest request.

Remove timeouts which were used to mitigate the racing issue but are obsolete now

Reviewed-by: Z
Refs: #123
```

---
## References

[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/#specification)



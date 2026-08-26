# FloridaPoly/.github

Organization-wide defaults for every repository in the `FloridaPoly` org.

This repository holds no application code. It exists so that issue intake,
security reporting, and contribution guidance are defined in one place instead
of being copied into each repository and drifting apart.

## What is here

| Path | Applies to |
| --- | --- |
| `.github/ISSUE_TEMPLATE/` | Every repo with no `ISSUE_TEMPLATE` folder of its own |
| `SECURITY.md` | Every repo with no `SECURITY.md` of its own |
| `ISSUE_STANDARD.md` | The written standard the forms implement |

## How inheritance works

GitHub applies a default file to a repository only when that repository has no
file of the same type. For issue templates the rule is all or nothing: if a
repository has **any** file in its own `.github/ISSUE_TEMPLATE` folder, none of
the defaults here are used in that repository.

Repositories that need their own intake form therefore carry the full set
locally, and are kept in step by script rather than by hand.

## Why this repository is public

GitHub does not support organization-wide default community health files from a
private or internal repository. Nothing here is sensitive: it is issue-intake
policy and a security-reporting route.

## Changing something

Open a pull request. A change here changes issue intake in roughly ninety-five
repositories at once, so treat wording as an interface.

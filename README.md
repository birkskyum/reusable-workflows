# reusable-workflows
This repository holds a list of shared workflows that are being used in other MapLibre repositories

## create-bump-version-pr.yml

This action is used to update package.json files with the relevant version (it receives a list of folders to run `npm version` in.
It is also used to add missing PRs to the changelog by looking at the git history, finding PR links, compare them to the existing changelog and add the missing ones.
It opens a PR with the relevant code changes to allow review, edit and merge.
Once merged, it relays on the following action to do the release.

## npm-publish.yml

This action is responsoble for both npm publishing and also publishing the docs to github pages for every release.
It start by checking if the version of the root package.json to see if it had changed.
If so, it starts by:
1. Installing and building the package.
2. Publishing it to npm.
3. Creating a git tag.
4. Creating a GitHub release.
5. Building the docs.
6. Publishing the docs to GitHub pages.

It releases from `main` by default. To release patches of an older major version from a maintenance branch, call it from that branch with `branch` set to the branch name, `npm-tag` set to an npm tag other than `latest` so that `latest` stays on the newest version, and `publish-docs` set to `false`.

## lint-pr-title.yml

This action checks that the PR title follows [Conventional Commits](https://www.conventionalcommits.org/) (a type such as `feat:` or `fix:`, then the description), using [action-semantic-pull-request](https://github.com/amannn/action-semantic-pull-request) with its defaults. It skips PRs opened by `github-actions[bot]`, like the one `create-bump-version-pr.yml` opens.

Call it from a workflow triggered by `pull_request_target`, so it also runs on PRs from forks. It checks out no code and needs only `pull-requests: read`.

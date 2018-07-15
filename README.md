# probot-auto-merge

[![Travis CI](https://travis-ci.org/bobvanderlinden/probot-auto-merge.svg?branch=master)](https://travis-ci.org/bobvanderlinden/probot-auto-merge)
[![Coverage](https://img.shields.io/coveralls/github/bobvanderlinden/probot-auto-merge.svg)](https://coveralls.io/github/bobvanderlinden/probot-auto-merge)

A GitHub App built with [Probot](https://github.com/probot/probot) that automatically merges PRs

## Usage

1. [Configure the GitHub App](https://github.com/apps/probot-auto-merge)
2. Create `.github/auto-merge.yml` in your repository.
3. Customize configuration to your needs. See below.

## Configuration

Configuration of probot-auto-merge is done through `.github/auto-merge.yml` in
your repository. An example of this file can be found [here](auto-merge.example.yml).
You can also see the configuration for this repository [here](.github/auto-merge.yml).

Note that the default configuration options are to do nothing. This is to prevent
impicit and possibly unintended behavior.

The configuration fields are as follows:

### `minApprovals` (required)

The minimum number of reviews from each association that approve the pull request before
doing an automatic merge. For more information about associations see:
https://developer.github.com/v4/reference/enum/commentauthorassociation/

Possible associations: `OWNER`, `MEMBER`, `COLLABORATOR`, `CONTRIBUTOR`, `FIRST_TIMER`, `FIRST_TIME_CONTRIBUTOR`, `NONE`

In the example below when a pull request gets 2 approvals from owners, members or collaborators,
the automatic merge will continue.

```yaml
minApprovals:
  COLLABORATOR: 2
```

In the example below when a pull request gets 1 approval from an owner OR 2 approvals from members, the automatic merge will continue.

```yaml
minApprovals:
  OWNER: 1
  MEMBER: 2
```

### `maxRequestedChanges`

Similar to `minApprovals`, maxRequestedChanges determines the maximum number of
requested changes before a pull request will be blocked from being automatically
merged.

It yet again allows you to configure this per association.

Note that `maxRequestedChanges` takes presedence over `minApprovals`.

In the example below, automatic merges will be blocked when one of the owners, members
or collaborators has requested changes.

```yaml
maxRequestedChanges:
  COLLABORATOR: 0
```

In the example below, automatic merges will be blocked when the owner has
requested changes or two members, collaborators or other users have requested
changes.

```yaml
maxRequestedChanges:
  OWNER: 0
  NONE: 1
```

The default for this value is:

```yaml
maxRequestedChanges:
  NONE: 0
```

### `updateBranch` (default: `false`)

Whether an out-of-date pull request is automatically updated.
It does so by merging its base on top of the head of the pull request.
This is similar to the behavior of the 'Update branch' button.

`updateBranch`` is useful for repositories where protected branches are used
and the option *Require branches to be up to date before merging* is enabled.

Note that this only works when the branch of the pull request resides in the same
repository as the pull request itself.

In the example below automatic updating of branches is enabled:

```yaml
updateBranch: true
```

### `deleteBranchAfterMerge` (default: `false`)

Whether the pull request branch is automatically deleted.
This is the equivalent of clicking the 'Delete branch' button shown on merged
pull requests.

Note that this only works when the branch of the pull request resides in the same
repository as the pull request itself.

In the example below automatic deletion of pull request branches is enabled:

```yaml
deleteBranchAfterMerge: true
```

### `mergeMethod` (default: `merge`)

In what way a pull request is merged. This can be:

* `merge`: creates a merge commit, combining the commits from the pull request on top of
  the base of the pull request (default)
* `rebase`: places the commits from the pull request individually on top of the base of the pull request
* `squash`: combines all changes from the pull request into a single commit and places the commit on top
  of the base of the pull request

For more information see https://help.github.com/articles/about-pull-request-merges/

```yaml
mergeMethod: merge
```

### `blockingLabels` (default: none)

Blocking labels are the labels that can be attached to a pull request to make
sure the pull request is not being merged automatically.

In the example below, pull requests that have the `blocked` label will not be
merged automatically.

```yaml
blockingLabels:
- blocked
```

### `requiredLabels` (default: none)

Whenever required labels are configured, pull requests will only be automatically
merged whenever all of these labels are attached to a pull request.

In the example below, pull requests need to have the label `merge` before they
will be automatically merged.

```yaml
requiredLabels:
- merge
```

## Development

### Setup

```sh
# Install dependencies
npm install

# Run typescript
npm run build
```

### Testing

```sh
npm run test
```

or during development:

```sh
npm run test:watch
```

### Running

See https://probot.github.io/docs/development/#configuring-a-github-app

```sh
npm run build && npm run dev
```

## Contributing

If you have suggestions for how probot-auto-merge could be improved, or want to report a bug, open an issue! We'd love all and any contributions.

For more, check out the [Contributing Guide](CONTRIBUTING.md).

## License

[ISC](LICENSE) © 2018 Bob van der Linden <bobvanderlinden@gmail.com> (https://github.com/bobvanderlinden/probot-auto-merge)

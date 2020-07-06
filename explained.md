# Renovate config explained

The [Renovate documentation](https://docs.renovatebot.com/) contains a lot of information.

For instance, [what the `config:base` setting means](https://docs.renovatebot.com/presets-config/).

According to the docs, it is:

```json
{
  "extends": [
    ":autodetectPinVersions",
    ":automergeDisabled",
    ":combinePatchMinorReleases",
    ":ignoreModulesAndTests",
    ":ignoreUnstable",
    ":prConcurrentLimit20",
    ":prHourlyLimit2",
    ":prImmediately",
    ":semanticPrefixFixDepsChoreOthers",
    ":separateMajorReleases",
    ":updateNotScheduled",

    "group:monorepos",
    "group:recommended",

    "helpers:disableTypesNodeMajor"
  ]
}
```

This config uses "presets". If you want to know more about presets, All presets
can be found at https://docs.renovatebot.com/presets-default/

Flattening the `extends` (ignoring `group:`'s, we'll get to that later) gives us:

```json
{
  "automerge": false,
  "ignorePaths": [
    "**/node_modules/**",
    "**/bower_components/**",
    "**/vendor/**",
    "**/examples/**",
    "**/__tests__/**",
    "**/test/**",
    "**/tests/**",
    "**/__fixtures__/**"
  ],
  "ignoreUnstable": true,
  "packageRules": [
    {
      "depTypeList": [
        "dependencies"
      ],
      "semanticCommitType": "fix"
    },
    {
      "packageNames": [
        "@types/node"
      ],
      "major": {
        "enabled": false
      }
    },
    {
      "packagePatterns": [
        "*"
      ],
      "semanticCommitType": "chore"
    }
  ],
  "prConcurrentLimit": 20,
  "prCreation": "immediate",
  "prHourlyLimit": 2,
  "rangeStrategy": "auto",
  "separateMajorMinor": true,
  "separateMinorPatch": false,
  "updateNotScheduled": true,
}
```

## What does it all _mean_?

Some of these options might be more obvious than others, so lets go through them
all.


    @TODO: Explain

## What would we like to change? (And why?)


### Automerge

When dependencies can be updated, Renovate created merge-request _by default_.
This means the merge-request still needs to be (manually) merged.
To change this, Renovate offers an auto-merge setting:

```json
  "automerge": true,
```

This will make Renovate merge any MR it opens, as long as it passes all checks.
If your project _doesn't_ have any checks, you need to explicitly tell Renovate
so, otherwise _nothing_ will get managed. This can be done with:

```json
  "requiredStatusChecks": null
```

If you don't want Renovate to open MRs _at all_, it can be told to merge
branches without opening an MR for for passing changes:

```json
  "automergeType": "branch"
```

If tests fail or remain pending for more than 24 hours, Renovate will still open
an MR to let you know something is wrong.

Things can be configured even further with `packageRules`. More on that later.

- - -

This configuration can be combined. They are ideal for small projects that do
not have (or does not care about) checks passing:

```json
  "automerge": true,
  "automergeType": "branch",
  "requiredStatusChecks": null
```

Just make sure to remove `requiredStatusChecks` when the project _does_ have
checks!

### Branch prefix

Any branch Renovate makes changes on starts with the prefix `renovate`. So, for
instance, if Renovate were to update ESLint, the branch name might be
`renovate/eslint-4.x`. Personally, I prefer the branch prefix for dependency
updates to be `dependency`. This can easily be configured:

```json
 "branchPrefix": "dependency"
```

### Dashboard

    !!! HIER GEBLEVEN !!! -> https://docs.renovatebot.com/configuration-options/#masterissue

### Description

This field is not needed for normal configuration. But, as we are making a
preset, it is helpful to describe what the preset is meant to do.

```json
  "description": "Config preset for small projects without CI checks."
```

The description field is used by config presets to. They are then collated as part of the onboarding description.

## Labels

If you actively use GitHub or GitLab issues, you probably also use labels to
order issues. By default, Renovate doesn't add any labels to the MRs it creates.
However, it can be told to do so:

```json
  "labels": ["dependencies"],
```

### Not pending

Another improvement is changing "prCreation" to `not-pending`. That way,
whenever you get a message that Renovate has opened an MR, you can be assured
all checks have already run. No more waiting on that "Pending" message!

## Anythings else?

The Renovate manual has [a section on noise reduction](https://docs.renovatebot.com/noise-reduction/)
that offers some good advise about reducing the manual and cognitive workload it
might create.

```json
{
  "extends": [
    ":maintainLockFilesMonthly"
    ":preserveSemverRanges",
    "group:all",
    "schedule:monthly",
  ],
  "lockFileMaintenance": {
    "extends": [
      "group:all"
    ],
    "commitMessageAction": "Update"
  },
  "separateMajorMinor": false
}
```

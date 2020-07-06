# Potherca Renovate config

![Renovate banner](./renovate-banner.jpg)

[![Renovate enabled](https://pother.ca/renovate-config/badge.svg)](https://renovate.whitesourcesoftware.com/)
[![standard-readme compliant](https://img.shields.io/badge/readme%20style-standard-green.svg)](https://github.com/RichardLitt/standard-readme)

_Renovate config presets for Potherca_

[Renovate](https://renovate.whitesourcesoftware.com/) is an automated solution to keep dependency up to date.

It is possible to create configuration for Renovate that can be [shared across repositories](https://docs.renovatebot.com/config-presets/).

This repository contains configuration files for Renovate to be used by Potherca projects.

<!-- @TODO: Add Table of Contents when README becomes longer than 100 lines -->

## Usage

Currently there is only one configuration file: `default.json`.

To use it, create a `.renovaterc.json` config file with this contents:

```
{
  "extends": ["github>Potherca/renovate-config"]
}
```

When Renovate is enabled for the repository with such a config file, the default config from this repo will be used.

To show it is using Renovate, the following can be added to the configured project's markdown README file:

```
[![Renovate enabled](https://pother.ca/renovate-config/badge.svg)](https://renovate.whitesourcesoftware.com/)
```

Which will produce this: [![Renovate enabled](https://pother.ca/renovate-config/badge.svg)](https://renovate.whitesourcesoftware.com/)

## Choice of config file location

Renovate will look in the following locations to find the config file:

- `.github/renovate.json`
- `.gitlab/renovate.json`
- `.renovaterc`
- `.renovaterc.json`
- `renovate.json`

The decision for which location to use was made as follows:

1. My convention for linters and other QA tools is to use the "hidden" file variant, with a dot `.` prefix.
   So the "regular" is not an option
2. The config file contains JSON, so the file without an extension is not an option.
3. To stay cross-vendor compatible, the GitHub and GitLab specific variants aren't an option

So the final decision is to use `.renovaterc.json`

<!--
## Configuration settings

    @TODO: Write details on cofiguration and link there
-->

## Contributing

Questions or feedback can be given by [opening an issue](https://github.com/Potherca/renovate-config/issues).

<!--
@TODO: Link to a CONTRIBUTING file
@TODO: Link to a Code of Conduct
-->

## License

This project has been published by [Potherca](https://pother.ca) under a [Mozilla Public License 2.0 (MPL-2.0)](./LICENSE).

<!--
SPDX-License-Identifier: Apache-2.0
SPDX-FileCopyrightText: 2025 The Linux Foundation
-->

# 🏷️ Check Calendar Versioning String/Tag

Validates a given string for conformity to Calendar Versioning.

Refer to the site/documentation: [Calendar Versioning](https://calver.org/)

## tag-validate-calver-action

## Usage Example

Pass the string to check as input to the action:

```yaml
steps:
  - name: "Check string for: CalVer"
    if: startsWith(github.ref, 'refs/tags/')
    uses: lfreleng-actions/string-validate-calver-action@main
    with:
      string: ${{ github.ref_name }}
```

## Inputs

<!-- markdownlint-disable MD013 -->

| Name         | Required | Default   | Description                                        |
| ------------ | -------- | --------- | -------------------------------------------------- |
| string       | True     | N/A       | Tag/version string to check for conformity         |
| exit_on_fail | False    | false     | Exits/aborts with error if check fails             |

<!-- markdownlint-enable MD013 -->

## Outputs

<!-- markdownlint-disable MD013 -->

| Name        | Description                                              |
| ----------- | -------------------------------------------------------- |
| valid       | Set true/false if string conforms to calendar versioning |

<!-- markdownlint-enable MD013 -->

## Implementation Details

There is no single fixed/agreed standard for calendar versioning, so this
action performs a simple regular expression test that a variety of different
calendar versioning implementations should pass.

For further details: <https://regex101.com/r/BQfFYn/1>

The RegEx used is:

`pattern="^(\d{2}|\d{4}).(\d{1}|\d{2})(\.(\d{1}|\d{2})?((\.|\-|\_)?)[a-zA-Z0-9].*)$"`

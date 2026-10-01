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

<!-- markdownlint-disable MD013 -->

```yaml
steps:
  - name: "Check string for: CalVer"
    if: startsWith(github.ref, 'refs/tags/')
    uses: lfreleng-actions/tag-validate-calver-action@51d2c429661b8040db2f4cc85a69184c5f64b297 # v0.1.3
    with:
      string: ${{ github.ref_name }}
```

<!-- markdownlint-enable MD013 -->

## Inputs

<!-- markdownlint-disable MD013 -->

| Name         | Required | Default   | Description                                        |
| ------------ | -------- | --------- | -------------------------------------------------- |
| string       | True     | N/A       | Tag/version string to check for conformity         |
| exit_on_fail | False    | false     | Exits/aborts with error if check fails             |

<!-- markdownlint-enable MD013 -->

## Outputs

<!-- markdownlint-disable MD013 -->

| Name        | Description                                                |
| ----------- | ---------------------------------------------------------- |
| valid       | Set true when tag/string conforms to Calendar Versioning   |
| dev_version | Set true when tag contains pre-release/development strings |

<!-- markdownlint-enable MD013 -->

## Implementation Details

There is no single fixed/agreed standard for calendar versioning, so this
action performs a simple regular expression test that a variety of different
calendar versioning implementations should pass.

The action removes a single leading `v` or `V` before the check, then
accepts strings with these parts, in order:

- a 2 or 4 digit year (`YY` or `YYYY`)
- a `.` followed by a 1 or 2 digit second segment (e.g. `MM` or `0M`)
- optionally, a `.` followed by a numeric micro segment
- optionally, one or more alphanumeric modifiers, each preceded by `.`, `-`
  or `_`

Examples that pass: `24.04`, `25.02.3`, `2025.02`, `2025.2.3`,
`2025.02.3-dev1`, `25.02.3-rc.1`, `2025.02.3_post1`

Examples that fail: `1.2.3`, `20250.3.4`, `2025x02.3.0`, `2025.02.3.`,
`2025.02-`

The check does not verify that the second segment is a valid month.

The RegEx used for calendar versioning compliance is:

`pattern="^(\d{2}|\d{4})\.(\d{1,2})(\.\d+)?([.\-_][0-9A-Za-z]+)*$"`

The RegEx used for development compliance is:

`pattern="(dev|pre|alpha|beta|rc|snapshot|nightly|canary|preview)"`

For further details: <https://regex101.com/r/d6KWNO/1>

The second RegEx uses the grep flags `"-Eqi"` which makes the match
case-insensitive and allows for partial matches.

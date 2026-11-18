<!-- markdownlint-disable -->

# Hardening Report: cpanato--vault-installer/v1.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **cpanato--vault-installer/v1.4.0** was hardened automatically. 26 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): the main composite run: block puts GitHub expressions straight into the bash script, where they are substituted before the shell runs. Examples: `mkdir -p ${{ inputs.install-dir }}` (line 40), `case ${{ runner.os }} in` (line 43), `pushd ${{ inputs.install-dir }}` (line 62), `case ${{ runner.arch }} in` (line 66), `desired_vault_filename='vault_${{ inputs.vault-release }}_linux_amd64.zip'` (line 68), `if "${{ inputs.use-sudo }}" == "true"` (line 121), `if [[ ${{ inputs.vault-release }} =~ $semver ]]` (line 127), `$SUDO curl -sL https://releases.hashicorp.com/vault/${{ inputs.vault-release }}/...` (lines 136-137), and `unzip ... -d "${{ inputs.install-dir }}"` (line 152). A crafted inputs.vault-release, install-dir or use-sudo value runs arbitrary shell commands. Several of these expansions are unquoted, and the version check happens only after the value has already been substituted into earlier lines.

Locations:

- `action.yml:40`
- `action.yml:43`
- `action.yml:62`
- `action.yml:121`
- `action.yml:127`
- `action.yml:136`

### script-injection (severity: high)

Rule (a): the Linux/macOS step's run: block uses an expression directly: `echo "${{ inputs.install-dir }}" >> $GITHUB_PATH`.

Locations:

- `action.yml:161`

### script-injection (severity: high)

Rule (a): the Windows pwsh step's run: block uses an expression directly: `echo "${{ inputs.install-dir }}" | Out-File -FilePath $env:GITHUB_PATH -Encoding utf8 -Append`.

Locations:

- `action.yml:164`

### github-env-injection (severity: high)

The Linux/macOS step writes inputs.install-dir straight to $GITHUB_PATH with no newline stripping (`printf '%s' ... | tr -d '\n\r'`): `echo "${{ inputs.install-dir }}" >> $GITHUB_PATH`. A value containing newlines adds extra PATH entries.

Locations:

- `action.yml:161`

### github-env-injection (severity: high)

The Windows step writes inputs.install-dir straight to $env:GITHUB_PATH with no newline stripping: `echo "${{ inputs.install-dir }}" | Out-File -FilePath $env:GITHUB_PATH ...`.

Locations:

- `action.yml:164`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.install-dir }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:40`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.install-dir }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:62`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vault-release }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:68`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vault-release }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:72`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vault-release }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:76`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vault-release }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:89`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vault-release }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:93`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vault-release }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:106`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.use-sudo }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:121`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vault-release }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:127`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vault-release }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:128`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vault-release }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:130`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vault-release }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:135`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vault-release }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:135`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vault-release }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:136`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vault-release }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:137`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vault-release }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:137`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.install-dir }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:152`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.vault-release }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:157`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.install-dir }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:162`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.install-dir }}" appears directly in run: block of step ""; move to env: map

Locations:

- `action.yml:165`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, github-env-injection, static-inline-injection

**Notes:**

All ${{ }} expressions in the run: blocks now go through step env: variables. The steps read inputs.vault-release, install-dir and use-sudo from those variables, and runner.os and runner.arch the same way.

- Main bash step: the version check now runs before vault-release is used anywhere. Paths, URLs and file names are double-quoted. The use-sudo check is now `[ "$INPUT_USE_SUDO" = "true" ]`. Before, the input's value was run as a command.
- install-dir: its default is the literal text '$HOME/.vault', which used to expand only because the expression was pasted straight into the script. To keep that working, a leading $HOME, ${HOME} or ~ is replaced with the real $HOME as plain text. Nothing is evaluated.
- PATH steps: the bash step and the pwsh step both remove newlines and carriage returns before writing to GITHUB_PATH. The pwsh step uses PowerShell's $HOME for the same replacement, which matches what the original pwsh step produced.

One known bug from upstream is left as is: the script sets `ret=$?` but then checks `$retVal`. Because the script runs with `set -e`, a failed checksum still stops the step before that check is reached.


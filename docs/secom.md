# SECOM

> Convention for Security Commit Messages.

🍵 Convention: https://tqrg.github.io/secom/<br>
💯 Compliance Checker: https://github.com/tqrg/secomlint

## Convention 🍵: SECOM

A convention for making security commit messages more readable and structured. Check the [CONFIG.md](https://github.com/TQRG/secom/blob/main/CONFIG.md) file to know how to configure the template in your repository.

```
1   vuln-fix: subject/header containing summary of changes in ~50 characters (Vuln-ID,)
2
3   Detailed explanation of the subject/header in ~75 words.
4   (what) Explain the security issue(s) that this commit is patching.
5   (why) Focus on why this patch is important and its impact.
6   (how) Describe how the issue is patched.
7
8   [For Each Weakness in Weaknesses:]
9   Weakness: weakness identification or CWE-ID.
10  Severity: severity of the  issue (Low, Medium, High, Critical).
11  CVSS: numerical representation (0-10) of the vulnerability severity.
12  Detection: method used to detect the issue (Tool, Manual, Exploit).
13  Report: http://link-to-report/
14  Introduced in: commit hash.
15  [End]
16
17  Reported-by: reporter name 1 <reporter-email-1@host.com>
18  Reported-by: reporter name 2 <reporter-email-2@host.com>
19  Signed-off-by: your name <your-email@yourhost.com>
20
21  [If you use an issue tracker, add reference to it here:]
22  [if external issue tracker:]
23  Bug-tracker: https://link-to-bug-tracker/id
24
25  [if github used as issue tracker:]
26  Resolves: #123
27  See also: #456, #789
```

### Details

This convention was inferred from merging different sources about creating better commits messages and from empirical research performed upon security commit messages.

```
<type>: <header/subject> (<Vuln-ID>)

<body>
# (what) describe the vulnerability/problem
# (why) describe its impact
# (how) describe the patch/fix

Weakness: <Weakness Name or CWE-ID>
Severity: <Low, Medium, High and Critical>
CVSS: <Numerical representation (0-10) of severity>
Detection: <Detection Method>
Report: <Report Link>
Introduced in: <Commit Hash>

Reported-by: <Name> (<Contact>)
Signed-off-by: <Name> (<Contact>)

Bug-tracker: <Bug-tracker Link>
OR
Resolves: <Issue/PR No.>
See also: <Issue/PR No.>
```

* Atomic changes: Commit each patch as a separate change [4].
* A `<type>` should be assigned to each commit [1]. Our suggestion is the usage of `vuln-fix to specify the fix is related to a vulnerability.
* `<header/subject>`: ~50 chars (max 72 chars); capitalized; no period in the end; imperative form.
* `<Vuln-ID>`: When available; e.g., CVE, OSV, GHSA, and other formats.
* `<body>`: Describe what (problem), why (impact) and how (patch). ~75 words (25 words per point).
* Weakness: Name or CWE-ID.
* Severity: Severity of the issue. Values: Low, Medium, High, Critical
* CVSS: Numerical (0-10) representation of the severity of a security vulnerability (Common Vulnerability Scoring System).
* Detection: Detection method. Values: Tool, Manual, Exploit, etc.
* Report: Link for vulnerability report.
* Introduced in: Commit hash from the commit that introduced the vulnerability.
* Reported-by: Name/Contact of the person that reported the issue.
* Signed-off-by: Name/Contact of the person that closed the issue.
* Bug-tracker: Link to the issue in an external bug-tracker.
* Resolves.. See also: When GitHub is used to manage security fixes.
  
In the future, we plan to infer the importance of each field and determine different levels of compliance. For now, we believe the following set of fields is the minimum required to detect and classify security commits succesfully: `<type>`, `<header/subject>`, `<Vuln-ID>`, `<body>`, Severity, Weakness.


## Compliance Checker 💯: SECOMlint

Linter to measure compliance against [SECOM](https://tqrg.github.io/secom/) convention. SECOM is a convetion for making security commit messages more readable and structured. Check the [CONFIG.md](https://github.com/TQRG/secom/blob/main/CONFIG.md) file to know how to configure the template in your repository.

<p align="center">
  <img width="800" src="https://raw.githubusercontent.com/TQRG/secomlint/main/assets/secomlint.svg">
</p>

### Installation

```
pip install secomlint
python -m spacy download en_core_web_lg
```

### Usage

```
secomlint --help
```
```
Usage: secomlint [OPTIONS]

  Linter to check compliance against SECOM (https://tqrg.github.io/secom/).

Options:
  --no-compliance        Show missing compliance.
  --is-body-informative  Checks body for security information.
  --score                Show compliance score.
  --config TEXT          Rule configuration file path name.
  --help                 Show this message and exit.
```

### Run tool

`git log -1 --pretty=%B | secomlint` where `git log -1 --pretty=%B` gets the commit message of the local commit.

<p align="center">
  <img width="800" src="https://raw.githubusercontent.com/TQRG/secomlint/main/assets/secomlint2.svg">
</p>

* Check only the rules that are not in compliance: `git log -1 --pretty=%B | secomlint --no-compliance`
* Calculate compliance score: `git log -1 --pretty=%B | secomlint --no-compliance --score`

### Configuration

The linter has a default configuration that can be overridden with a `.yml` file using the following syntax: 

```
rule_name:
    active: {true | false}
    type: {0 - warning | 1 - error}
    value: {string | regex}
```

An example would be:

```
header_starts_with_type:
  active: true
  type: 0
  value: 'fix'
metadata_has_detection:
  active: false
```
The rule `header_starts_with_type` is active, outputs warnings and checks if header starts with type `fix`. The rule `metadata_has_detection` was deactivated.

```
git log -1 --pretty=%B | secomlint --config=config.yml
```

### Check if the message's body is informative

It is important that the body of security commit messages are somehow informative; `secomlint` checks your body for security-related keywords.

```
git log -1 --pretty=%B | secomlint --is-body-informative
```
```
👍 Good to go! Extractor found the following security related words in the message's body:
   - protocols
```
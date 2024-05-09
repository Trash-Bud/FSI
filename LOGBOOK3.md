# Week #3: Git Credential Manager Core Vulnerability (CVE-2020-26233)

## Identification:

- Git Credential Manager Core (GCM Core) is a secure Git credential helper that runs on Windows and macOS;
- Before version 2.0.289, if a malicious git.exe executable is present in the top-level repository this binary will be started by it when attempting to read configuration, and not git.exe as found on the %PATH%;
- This only affects GCM Core on Windows, not macOS or Linux-based distributions;
- Use of incorrectly-resolved name or reference exploit (CWE-706).

## Cataloguing:

- This exploit was given a CVSS 2.0 score of 3.6 (Low), and a CVSS 3.X score of 7.3 (High);
- It was found by studying committed code from GitHub CLI 1.2.1 release;
- Credit for reporting the vulnerability goes to Vitor Fernandes from Blaze Information Security;
- It was published on 12/08/2020.

## Exploit:

- When forking a new private repository, a remote code execution scenario was possible after cloning;
- The command git.exe config credential.namespace is called without the *safeexec.LookPath()* function, therefore Windows will fallback to its default and search for the git.exe binary in the current cloned repository;
- *safeexec.LookPath()* provides an alternative to *exec.LookPath()* which has the unwanted behaviour of looking for the git.exe file in the current directory before searching folders listed in the %PATH% environment variable leading to git.exe and git.bat files in the current directory to be run instead.

## Attacks:

- There have been no reported attacks using this vulnerability as it was discovered and reported before the possibility of it being being exploited;
- The supposed attack would go as follows:
  1. the attacker would get access to a git repository where they could add files;
  2. the attacker would upload a Windows executable to the root directory with the name “git.exe”;
  3. as the victim forked the repository the malicious code within the git.exe file would be executed.

## Useful links:

- https://nvd.nist.gov/vuln/detail/CVE-2020-26233#vulnCurrentDescriptionTitle
- https://blog.blazeinfosec.com/attack-of-the-clones-2-git-command-client-remote-code-execution-strikes-back/
- https://github.com/cli/cli/releases/tag/v1.2.1
- https://github.com/cli/safeexec

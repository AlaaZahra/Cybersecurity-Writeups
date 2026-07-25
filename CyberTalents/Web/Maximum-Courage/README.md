\# Maximum Courage



> \*\*Platform:\*\* CyberTalents  

> \*\*Category:\*\* Web Security  

> \*\*Difficulty:\*\* Easy  

> \*\*Vulnerability:\*\* Sensitive Data Exposure (Exposed Git Repository)



\---



\# Overview



This challenge demonstrates how an exposed Git repository can lead to sensitive source code disclosure.



Instead of attacking the application directly, the objective is to enumerate hidden resources, identify an exposed `.git` directory, reconstruct the repository, and inspect the application's source code to retrieve the hidden secret.



\---



\# Objective



Recover the application's source code and obtain the hidden flag by exploiting an exposed Git repository.



\---



\# Environment



\*\*Operating System\*\*



\- Windows 11



\*\*Terminal\*\*



\- PowerShell



\*\*Tools\*\*



\- dirsearch

\- Git Dumper

\- Git

\- Python

\- Web Browser



\---



\# Methodology



\## 1. Environment Setup



The required tools were installed and configured before starting the assessment.



\### Installing Dirsearch



```powershell

git clone https://github.com/maurosoria/dirsearch.git



cd dirsearch



py -m pip install -r requirements.txt

```



!\[Dirsearch Installation](images/setup-dirsearch.png)



\---



\## 2. Directory Enumeration



The target was enumerated using \*\*dirsearch\*\* to discover hidden files and directories.



```powershell

py dirsearch.py -u http://TARGET/

```



Several interesting resources were discovered, including an exposed Git repository.



Notable findings:



\- `/.git/`

\- `/.git/config`

\- `/.git/index`

\- `/.git/HEAD`

\- `/flag.php`



This immediately indicated that the application's Git repository was publicly accessible.



!\[Directory Enumeration](images/dirsearch.png)



\---



\## 3. Repository Reconstruction



Since the Git metadata was accessible, the repository could be reconstructed locally using \*\*Git Dumper\*\*.



```powershell

git-dumper http://TARGET/.git/ maximum-courage-repo

```



The tool successfully downloaded the repository and restored the tracked files.



!\[Git Dumper](images/git-dumper.png)



\---



\## 4. Source Code Review



After reconstructing the repository, the application files became available for analysis.



Reviewing the recovered `flag.php` source code revealed the hidden secret that was not accessible through the web application itself.



```php

<?php



exit();

die();



$secret\_key = "\*\*\*\*\*\*\*\*";



?>

```



The PHP interpreter executes the script before sending a response to the client.



However, downloading the exposed Git repository provides direct access to the original source code, bypassing normal server-side execution.



> \*\*Note:\*\* The secret value has been intentionally removed to avoid publishing the challenge solution.



!\[Recovered Source Code](images/source-code.png)



\---



\# Root Cause



The web server exposed the `.git` directory.



As a result, attackers were able to:



\- Download Git metadata.

\- Reconstruct the repository.

\- Access application source code.

\- Recover sensitive information stored inside the project.



\---



\# Mitigation



\- Block public access to `.git`.

\- Remove Git repositories before deployment.

\- Store secrets using environment variables.

\- Avoid hardcoding credentials inside source files.

\- Perform regular security assessments to detect exposed resources.



\---



\# Lessons Learned



\- Enumeration is one of the most important phases during web penetration testing.

\- Hidden directories should never be ignored.

\- An exposed Git repository may completely disclose an application's source code.

\- Source code disclosure often reveals significantly more information than the web interface itself.



\---



\# Skills Gained



\- Web Enumeration

\- Directory Discovery

\- Git Repository Analysis

\- Source Code Review

\- Sensitive Data Exposure

\- Security Misconfiguration Analysis



\---



\# References



\- CyberTalents

\- Git Dumper

\- Dirsearch

\- OWASP - Sensitive Data Exposure



\---



\# Disclaimer



This write-up is intended for educational purposes only.



All testing was performed against a legally accessible Capture The Flag (CTF) environment provided by CyberTalents.


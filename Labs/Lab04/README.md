# Lab 4: Simple Python Database Connections

**Student:** faisal-yusra
**Course:** CS-649 Cloud DevOps
**Databases fork:** https://github.com/faisal-yusra/Databases

## Contents
- `Lab4_Solution.pdf` - connection results, explanations and evidence
- `screenshots/` - the required Lab 4 screenshots

## Summary
One file, `db_connect.py`, was edited through four stages:
1. MySQL with values written in the code
2. MongoDB with values written in the code
3. MongoDB with values read using `os.getenv()`
4. MongoDB with values read from a credentials file (`configparser`)

Python ran directly on the Ubuntu server. Docker ran only the databases
(MySQL and three MongoDB deployments: dev, test and simulated prod).
`credentials.ini` stayed local and was not committed.

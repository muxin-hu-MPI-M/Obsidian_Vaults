---
tags:
  - code
  - code/tips
Last Eddited: 2025-12-17
---

- to check the size of individual file in the directory: `ls -ahlrS --color=auto`
- to get access to individual compute node: 
```
salloc --partition=compute --nodes=1 --time=08:00:00 --account mh0033
```
- to have the log file when running a script/command
```
2>&1 | tee XXX.log
```
- to check the difference between the local commits to the merged one
```
git difftool --tool=meld -d origin/icon-2026.10
```
- to check the updating log file with certain words, `Ctrl+C` to stop
```
tail -n 50 -F xxx | grep --line-buffered 'xxx'
```
- To upload the quick plots output:
```
cd pyicon/all_qps
module load py-python-swiftclient

# if the swift token is expired 
swift-token new
Account: mh0033
Username: m301254
Password: same for levante

# if the token is there
cd all_qps
swift upload m301254 *

# the website: 
```

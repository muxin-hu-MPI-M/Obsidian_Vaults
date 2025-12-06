---
tags:
  - project/surfwaves
  - ICON
Last Eddited: 2025-11-27T09:57:00
---


# Off branch
## Compare to master branch
remember, the current git branch with XPP mkexp configuration is the off branch in `icon-XPP-20250826`, which is somehow older version since the ICON is continuing developing

Check:
```shell
(base) bash-4.4$ git rev-list --left-right --count remotes/origin/HEAD...icon-XPP-20250806
	244     55
```
this means:
- Your XPP branch is **55 commits ahead** of its base,  
- and **244 commits behind the latest ICON development**.
	- It means XPP is a **snapshot + extensions**

## Inside the XPP branch
Upon [[2025-12-06]], my git version is up-to-date with the latest version
Check:
```shell
(base) bash-4.4$ git status
	On branch icon-XPP-20250806
	Your branch is up to date with 'origin/icon-XPP-20250806'.
	
	Untracked files:
	  (use "git add <file>..." to include in what will be committed)
	        ../../../run/mux0001_b5b7.config
```

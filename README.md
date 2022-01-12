# Submodules
  Learning how to include and pin specific submodules.
  
  We'll be utilizing the biotools suite to do this

---
## Create submodules

From inside a git repo, add submodules with `git submodule add`, ensure you use a reference that all users will be able to reach.  
```bash
git submodule add --name bcftools -b master git@github.com:samtools/bcftools.git bcftools
git submodule add --name samtools -b master git@github.com:samtools/samtools.git samtools
git submodule add --name singularity -b master https://github.com/apptainer/singularity.git singularity
```
Check the status
```bash
(main) % git submodule add --name samtools -b master git@github.com:samtools/samtools.git samtools
Cloning into './samtools'...
remote: Enumerating objects: 12626, done.
remote: Counting objects: 100% (809/809), done.
remote: Compressing objects: 100% (312/312), done.
remote: Total 12626 (delta 563), reused 672 (delta 494), pack-reused 11817
Receiving objects: 100% (12626/12626), 13.45 MiB | 1.85 MiB/s, done.
Resolving deltas: 100% (8482/8482), done.

submodules (main 2S-0U) % git submodule add --name bcftools -b master git@github.com:samtools/bcftools.git bcftools
Cloning into './bcftools'...
remote: Enumerating objects: 14943, done.
remote: Counting objects: 100% (1440/1440), done.
remote: Compressing objects: 100% (787/787), done.
remote: Total 14943 (delta 878), reused 1104 (delta 625), pack-reused 13503
Receiving objects: 100% (14943/14943), 17.51 MiB | 1.60 MiB/s, done.
Resolving deltas: 100% (9840/9840), done.

submodules (main 4S-0U) % git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   .gitmodules
	new file:   bcftools
	new file:   samtools
	new file:   singularity

submodules (main 4S-0U) % git submodule status
 5f1bf7a1b016c24d38657bdde5fd2ca27e6954e9 bcftools (1.14)
 c29621d3ae075573fce83e229a5e02348d4e8147 samtools (1.14)
 2f16701e33f5e54824429d196b239cff30e208be singularity (v3.8.3-222-g2f16701e3)

submodules (main 4S-0U) % cat .gitmodules
[submodule "samtools"]
	path = samtools
	url = git@github.com:samtools/samtools.git
	branch = master
[submodule "bcftools"]
	path = bcftools
	url = git@github.com:samtools/bcftools.git
	branch = master
[submodule "singularity"]
	path = singularity
	url = https://github.com/apptainer/singularity.git
	branch = master
```

### Associate a specific tag (or revision) of a submodule with a branch
To get specific revisions of a submodule into the branch, you'll need to go to each submodule and checkout the tag. <br>
Since each submodule is tracked commit revision, it will always show as detached.<br>
Don't forget to push your tags!


```bash
submodules (main) $ git checkout -b tagging
Switched to a new branch 'tagging'

submodules (tagging <>) $ cd bcftools/

bcftools ((detached) <>) $ git checkout tags/1.0
Previous HEAD position was 8bf3593 bugfix: missing "," in ALT
HEAD is now at 620cc50 Release 1.0: first BCFtools release

bcftools ((detached) <>) $ cd ../samtools/

samtools ((detached) <>) $ git checkout tags/1.0
Previous HEAD position was c29621d Release 1.14
HEAD is now at 5370fe9 Release 1.0: first htslib-based samtools release
samtools ((detached) <>) $ cd ../singularity/

singularity ((detached) <>) $ git checkout tags/1.0
Previous HEAD position was 2f16701e3 Merge pull request #6328 from DrDaveD/disable-dependabot
HEAD is now at 932107407 Fixes for Debian/*ubuntu

singularity ((detached) <>) $ cd ..

submodules (tagging <> 0S-3U) $ git diff
diff --git a/bcftools b/bcftools
index 5f1bf7a..620cc50 160000
--- a/bcftools
+++ b/bcftools
@@ -1 +1 @@
-Subproject commit 5f1bf7a1b016c24d38657bdde5fd2ca27e6954e9
+Subproject commit 620cc50ffa5a41f2cdfd8c629ec6179bcb05785c
diff --git a/samtools b/samtools
index c29621d..5370fe9 160000
--- a/samtools
+++ b/samtools
@@ -1 +1 @@
-Subproject commit c29621d3ae075573fce83e229a5e02348d4e8147
+Subproject commit 5370fe9d469c8b123bec1638dd1e3d2f92308bd6
diff --git a/singularity b/singularity
index 2f16701..9321074 160000
--- a/singularity
+++ b/singularity
@@ -1 +1 @@
-Subproject commit 2f16701e33f5e54824429d196b239cff30e208be
+Subproject commit 9321074074ca00384322bdf3373c3701f4c6d3d2

submodules (tagging <> 0S-3U) $ git commit -am "version 1.0 of all tools"
[tagging 48345dc] version 1.0 of all tools
 3 files changed, 3 insertions(+), 3 deletions(-)

submodules (tagging <>) $ git tag 1.0 -am "versions 1.0 of all tools"

submodules (tagging <>) $  git push --set-upstream origin tagging
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Delta compression using up to 16 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 409 bytes | 409.00 KiB/s, done.
Total 2 (delta 0), reused 0 (delta 0), pack-reused 0
remote:
remote: Create a pull request for 'tagging' on GitHub by visiting:
remote:      https://github.com/helix-clyde/submodules/pull/new/tagging
remote:
To github.com:helix-clyde/submodules.git
 * [new branch]      tagging -> tagging
Branch 'tagging' set up to track remote branch 'tagging' from 'origin'.

submodules (tagging) $ git push --tags
Enumerating objects: 2, done.
Counting objects: 100% (2/2), done.
Delta compression using up to 16 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 326 bytes | 326.00 KiB/s, done.
Total 2 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:helix-clyde/submodules.git
 * [new tag]         1.0 -> 1.0

```

---
## Cloning
Just doing a git clone will *not* pull the submodules<br>
Running `git clone git@github.com:helix-clyde/submodules.git` will pull the repository, but will not checkout the submodules e.g.

```bash
git clone git@github.com:helix-clyde/submodules.git submodules_bare
Cloning into 'submodules_bare'...
remote: Enumerating objects: 13, done.
remote: Counting objects: 100% (13/13), done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 13 (delta 1), reused 7 (delta 1), pack-reused 0
Receiving objects: 100% (13/13), 14.95 KiB | 464.00 KiB/s, done.
Resolving deltas: 100% (1/1), done.
$ cd submodules_bare/

submodules_bare (main) $ ll
total 52
-rw-r--r-- 1 clyde.jones docker 35149 Jan 11 12:57 LICENSE
-rw-r--r-- 1 clyde.jones docker  2176 Jan 11 12:57 README.md
drwxr-xr-x 2 clyde.jones docker  4096 Jan 11 12:57 bcftools
drwxr-xr-x 2 clyde.jones docker  4096 Jan 11 12:57 samtools
drwxr-xr-x 2 clyde.jones docker  4096 Jan 11 12:57 singularity

submodules_bare (main) $ git submodule status
-5f1bf7a1b016c24d38657bdde5fd2ca27e6954e9 bcftools
-c29621d3ae075573fce83e229a5e02348d4e8147 samtools
-2f16701e33f5e54824429d196b239cff30e208be singularity

submodules_bare (main) $ cd bcftools/
submodules_bare/bcftools (main) $ ll
total 0
submodules_bare/bcftools (main) $
```

To pull the submodule code if you've done a plain clone, do:
```bash
git submodule update --init samtools/
Submodule 'samtools' (git@github.com:samtools/samtools.git) registered for path 'samtools'
Cloning into 'submodules_bare/samtools'...
Submodule path 'samtools': checked out 'c29621d3ae075573fce83e229a5e02348d4e8147'
```

or you can run this bash snippet
```
for subm in $(grep submodule .gitmodules  | cut -d \" -f 2);
  do
    git submodule update --init $subm
  done
```
Run `git clone --recursive git@github.com:helix-clyde/submodules.git` to do the full checkout

---
## Changing branches

The sha tag of the branch is tracked in `$(git root)/.gitmodules` and in the configuration directory `.git/modules/<submodulename>/HEAD`

When you change to a branch that has a different submoddule commit, the submodules will **not** change to the committed version.

In order to change to the commited version of the submodule run
```bash
git submodule update --checkout --recursive
```
Note the following example
```bash
submodules (main) $ git submodule
 5f1bf7a1b016c24d38657bdde5fd2ca27e6954e9 bcftools (1.14)
 c29621d3ae075573fce83e229a5e02348d4e8147 samtools (1.14)
 2f16701e33f5e54824429d196b239cff30e208be singularity (v3.8.3-222-g2f16701e3)

submodules (main) $ git checkout 1.13
M	bcftools
M	samtools
M	singularity
Switched to branch '1.13'
Your branch is up to date with 'origin/1.13'.

submodules (main) $ git diff
diff --git a/bcftools b/bcftools
index fc13b08..5f1bf7a 160000
--- a/bcftools
+++ b/bcftools
@@ -1 +1 @@
-Subproject commit fc13b08a45e0577c01a831412bbd3cd5119866a7
+Subproject commit 5f1bf7a1b016c24d38657bdde5fd2ca27e6954e9
diff --git a/samtools b/samtools
index bcd9f35..c29621d 160000
--- a/samtools
+++ b/samtools
@@ -1 +1 @@
-Subproject commit bcd9f35cefdd8b92044d2259506aff39f0a6baf9
+Subproject commit c29621d3ae075573fce83e229a5e02348d4e8147
diff --git a/singularity b/singularity
index 6177403..2f16701 160000
--- a/singularity
+++ b/singularity
@@ -1 +1 @@
-Subproject commit 61774037330ebcdd1852f381d4f9f5acf5230045
+Subproject commit 2f16701e33f5e54824429d196b239cff30e208be
submodules (main) $ git submodule
+5f1bf7a1b016c24d38657bdde5fd2ca27e6954e9 bcftools (1.14)
+c29621d3ae075573fce83e229a5e02348d4e8147 samtools (1.14)
+2f16701e33f5e54824429d196b239cff30e208be singularity (v3.8.3-222-g2f16701e3)

submodules (main) $ git submodule update --checkout --recursive
Submodule path 'bcftools': checked out 'fc13b08a45e0577c01a831412bbd3cd5119866a7'
Submodule path 'samtools': checked out 'bcd9f35cefdd8b92044d2259506aff39f0a6baf9'
Submodule path 'singularity': checked out '61774037330ebcdd1852f381d4f9f5acf5230045'

submodules (main) $ git submodule
 fc13b08a45e0577c01a831412bbd3cd5119866a7 bcftools (1.13)
 bcd9f35cefdd8b92044d2259506aff39f0a6baf9 samtools (1.13)
 61774037330ebcdd1852f381d4f9f5acf5230045 singularity (v3.8.5)

```

---
## References
 - Official Git References
   - https://git-scm.com/book/en/v2/Git-Tools-Submodules
   - https://git-scm.com/docs/git-submodule

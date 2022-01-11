# Testing submodules
  Learning how to include and pin specfic submodules.
  We'll be utilizing biotools to do this 
  
  Create submodules 
  ```bash
  git submodule add --name bcftools -b master git@github.com:samtools/bcftools.git bcftools
  git submodule add --name samtools -b master git@github.com:samtools/samtools.git samtools
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

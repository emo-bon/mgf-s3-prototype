# mgf-s3-prototype
## Prerequisites
- [Get Started with DVC](https://dvc.org/doc/start)
- `~/.aws` folder with S3 config and credentials
```
conda install -c conda-forge dvc dvc-s3
conda install -c conda-forge s5cmd  # optional install in case you want direct access to the s3 bucket
```
## Use case 1: initializing a new analysis-results-cluster
Before running the following commands, it is assumed that you already ran `git init` or `git clone`d an existing repository.

```
dvc init
git commit -m "initialize dvc"
dvc remote add -d myremote s3://emobon-lfs-demo
dvc remote modify myremote endpointurl https://s3.mesocentre.uca.fr
dvc remote modify myremote profile "eosc-fairease1" # run this if you make use of a profile in your s3 config
git add .dvc/config
git commit -m "update dvc config"
```

## Use case 2: pushing large files
Copy the new files you want to add into the repository on your local machine and start by adding the large files to DVC.

For example:

```
dvc add mgf-crate-0/results/DBH_AAAGOSDA_1_1_HMNJKDSX3.UDI200_clean.fastq.trimmed.fasta
dvc add mgf-crate-0/results/DBH_AAAGOSDA_1_2_HMNJKDSX3.UDI200_clean.fastq.trimmed.fasta
dvc add mgf-crate-0/results/DBH.merged_CDS.faa
...
```

Once all large files have been added to DVC, the `.dvc` placeholder files and `.gitignore` can be committed and the large files can be pushed to the S3 bucket.

```
git add mgf-crate-0/results/*.dvc mgf-crate-0/results/.gitignore
git commit -m "add large files"
dvc push
```

The remaining small files can then be committed directly.

```
git add .
git commit -m "add small files"
git push origin
```

## Use case 3: pulling large files
A single command is sufficient to retrieve all large files in one go.

```
dvc pull
```

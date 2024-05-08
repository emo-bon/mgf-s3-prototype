# mgf-s3-prototype
## prerequisites
- `~/.aws` folder with s3 config and credentials
```
# conda install -c conda-forge s5cmd
conda install -c conda-forge dvc dvc-s3
```

## pushing large files
```
dvc init
git commit -m "Initialize DVC"
dvc add mgf-crate-0/results/DBH.merged.fasta
...
git add mgf-crate-0/results/*.dvc mgf-crate-0/results/.gitignore
git commit -m "add large files"
dvc remote add -d myremote s3://emobon-lfs-demo
dvc remote modify myremote endpointurl https://s3.mesocentre.uca.fr
# dvc remote modify myremote profile "eosc-fairease1"
git add .dvc/config
git commit -m "update dvc config"
dvc push
git add .
git commit -m "add small files"
```

## pulling large files
```
dvc pull
```

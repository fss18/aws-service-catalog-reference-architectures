# Prerequisites
Launch Cloud9 and run everything via Cloud9
Select:
* Create a new instance for environment (EC2)
* t2.micro (1 Gib RAM + 1 vCPU)
* Amazon Linux

Open the Cloud9 IDE and use the terminal to run all commands

# Building Pipeline

Clone the following repository
```
git clone https://github.com/fss18/aws-service-catalog-reference-architectures
```

Navigate to the cloned repository
```
cd aws-service-catalog-reference-architectures/codepipeline/
```

Modify the bash script permissions
```
sudo chmod u+x ct_install_multi.sh
```

Run the command below, change `<alias>` into your own customized alias
```
./ct_install_multi.sh <alias> yes
```

# Using CodeCommit
Navigate to the home directory
```
cd ../..
```

Enable CodeCommit credential helper
```
git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true
```

From the AWS Console, navigate to the CodeCommit, and copy the repository URL (HTTPS)
Clone the repository:
```
git clone <enter your repository HTTPS URL>
```

Navigate to your empty repository
```
cd <alias>-SCPortfoliosRepo
```

Copy all files from the previous repository to your new repository
```
cp -r ../aws-service-catalog-reference-architectures/* .
```

From Cloud9 console, find these two files:
```
codepipeline/buildspec-multi.yml
codepipeline/run-pipelineupdate-multi.sh
```
Replace the line: `ALIAS=initial` to `ALIAS=<your-alias>`

Add all files to the repo, commit and push
```
git add *
git commit -a -m ‘Initial clone of the aws-service-catalog-reference-architectures repository’
git push
```

# Modifying Product Catalog

Navigate to the `ec2` folder in your alias-SCPortfoliosRepo.
Modify the line in `sc-ec2-linux-apache-simple.json` that says "Congratulations, you have…" to "Congratulations alias@, you have…"
Modify `sc-product-ec2-webserver.json` to change the name of the product to "Apache v1.0 – alias@"
```
{
    "Description": "Apache webserver",
    "Info": {
        "LoadTemplateFromURL": {"Fn::Sub": "${RepoRootURL}ec2/sc-ec2-linux-apache-simple.json"}
    },
    "Name": "Apache v1.0 - <your alias>"
}
```

Push changes to repo
```
git commit –a –m ‘silly modification to sc-ec2-linux-apache-simple.json’
git push
```

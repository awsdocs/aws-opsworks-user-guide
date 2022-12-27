# Step 2: Download the migration script<a name="migrating-to-systems-manager-download-script"></a>

Download the zip file containing the migration script and all relevant files by running the following command\.

```
aws s3api get-object \
    --bucket export-opsworks-stacks-bucket-prod-us-east-1 \
    --key export_opsworks_stacks_script.zip export_opsworks_stacks_script.zip
```

If you are using Linux, install the unzip utility using the following commands\.

```
sudo apt-get install unzip
sudo yum install unzip
```

Unzip the files using the appropriate command for your operating system\.

**For Linux**, use the following command\.

```
unzip export_opsworks_stacks_script.zip
```

**For Windows**, use the `Expand-Archive` command in PowerShell\.

```
Expand-Archive -LiteralPath PathToZipFile -DestinationPath PathToDestination
```

After the file is unzipped, the following directories and files are available\.
+ README\.md
+ LICENSE 
+ NOTICE
+ requirements\.txt
+ templates/
  + OpsWorksCFNTemplate\.yaml
  +  MountEBSVolumes\.yaml 
+ opsworks/
+ cloudformation/
+ instances\_tab/
+ cfn\_stack\_deployer\.py 
+ s3\.py
+ stack\_exporter\_context\.py
+ stack\_exporter\.py
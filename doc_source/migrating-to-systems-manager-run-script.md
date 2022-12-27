# Step 4: Run the script<a name="migrating-to-systems-manager-run-script"></a>

When you run `python3 stack_exporter.py`, you can either provision the application, or create a starter template by setting the value of the `--provision-application` parameter to `FALSE`\.

**Example 1: Provision a Systems Manager Application Manager application**

The following command gets information about an existing OpsWorks layer, and provisions an application using the newer OpsWorks architecture, which achieves a result similar to the Chef version configured for the stack\. The script provisions all required resources, such as Auto Scaling groups by using CloudFormation, and then registers the application in Systems Manager Application Manager\.

Replace *stack\-region* and *layer\-id* with the values for your OpsWorks stack and layer\. 

```
python3 stack_exporter.py \
     --layer-id layer-id \
     --region stack-region
```

**Example 2: Generate a template**

The following command gets information about an existing OpsWorks layer and generates a CloudFormation template\. The template, if provisioned, achieves a result similar to using Chef 14\. In this example, no resources are provisioned, because the `--provision-application` parameter is set to `FALSE`\.

Replace *stack\-region* and *layer\-id* with the values for your OpsWorks stack and layer\. 

```
python3 stack_exporter.py \
    --layer-id layer-id \
    --region stack-region \
    --provision-application FALSE
```

After running the command, you can review the template in the Application Manager template library in Systems Manager, and you can also provision the template\. For more information about viewing the template library, see [Working with the template library](https://docs.aws.amazon.com/systems-manager/latest/userguide/application-manager-working-templates-overview.html#application-manager-working-stacks-template-library-working) in the *AWS Systems Manager User Guide*\.
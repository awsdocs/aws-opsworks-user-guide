# AWS OpsWorks Stacks Operating Systems<a name="workinginstances-os"></a>

AWS OpsWorks Stacks supports the 64\-bit versions of several built\-in operating systems, including Amazon and Ubuntu Linux distributions, and Microsoft Windows Server\. Some general notes:
+ A stack's instances can run either Linux or Windows\.

  A stack can have different Linux versions or distributions on different instances, but you cannot mix Linux and Windows instances\.
+ You can use [custom AMIs](workinginstances-custom-ami.md) \(Amazon Machine Images\), but they must be based on one of the AWS OpsWorks Stacks\-supported AMIs that are described in topics in this section\. While it might be possible to create or register instances with other operating systems \(such as CentOS 6\.*x*\) that have been created from custom or community\-generated AMIs, these are not officially supported\.
  + [Linux Operating Systems](workinginstances-os-linux.md)
  + [Microsoft Windows Server](workinginstances-os-windows.md)
+ You can [start and stop instances manually](workinginstances-starting.md) or have AWS OpsWorks Stacks [automatically scale](workinginstances-autoscaling.md) the number of instances\.

  You can use time\-based automatic scaling with any stack; Linux stacks also can use load\-based scaling\.
+ In addition to using AWS OpsWorks Stacks to create Amazon EC2 instances, you can also [register instances with a Linux stack](workinginstances-autoscaling.md) that were created outside of AWS OpsWorks Stacks\.

  This includes Amazon EC2 instances and instances running on your own hardware\. However, they must be running one of the supported Linux distributions\. You cannot register Amazon EC2 or on\-premises Windows instances\.

You can use the AWS OpsWorks Stacks [https://docs.aws.amazon.com/opsworks/latest/APIReference/API_DescribeOperatingSystems.html](https://docs.aws.amazon.com/opsworks/latest/APIReference/API_DescribeOperatingSystems.html) API to return a list of supported operating systems and their supported versions of Chef\. The following is an example command, using the AWS CLI\.

```
aws opsworks describe-operating-systems
```

The following is an example response\.

```
{
    "OperatingSystems": [
        {
            "Name": "Amazon Linux",
            "Type": "Linux",
            "Supported": false,
            "ReportedVersion": "2014.03",
            "ReportedName": "amazon",
            "Id": "Amazon Linux",
            "ConfigurationManagers": [
                {
                    "Version": "11.10",
                    "Name": "Chef"
                },
                {
                    "Version": "11.4",
                    "Name": "Chef"
                },
                {
                    "Version": "0.9",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "Amazon Linux 2014.09",
            "Type": "Linux",
            "Supported": false,
            "ReportedVersion": "2014.09",
            "ReportedName": "amazon",
            "Id": "Amazon Linux 2014.09",
            "ConfigurationManagers": [
                {
                    "Version": "11.10",
                    "Name": "Chef"
                },
                {
                    "Version": "11.4",
                    "Name": "Chef"
                },
                {
                    "Version": "0.9",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "Amazon Linux 2015.03",
            "Type": "Linux",
            "Supported": false,
            "ReportedVersion": "2015.03",
            "ReportedName": "amazon",
            "Id": "Amazon Linux 2015.03",
            "ConfigurationManagers": [
                {
                    "Version": "12",
                    "Name": "Chef"
                },
                {
                    "Version": "11.10",
                    "Name": "Chef"
                },
                {
                    "Version": "11.4",
                    "Name": "Chef"
                },
                {
                    "Version": "0.9",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "Amazon Linux 2015.09",
            "Type": "Linux",
            "Supported": false,
            "ReportedVersion": "2015.09",
            "ReportedName": "amazon",
            "Id": "Amazon Linux 2015.09",
            "ConfigurationManagers": [
                {
                    "Version": "12",
                    "Name": "Chef"
                },
                {
                    "Version": "11.10",
                    "Name": "Chef"
                },
                {
                    "Version": "11.4",
                    "Name": "Chef"
                },
                {
                    "Version": "0.9",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "Amazon Linux 2016.03",
            "Type": "Linux",
            "ReportedVersion": "2016.03",
            "ReportedName": "amazon",
            "Id": "Amazon Linux 2016.03",
            "ConfigurationManagers": [
                {
                    "Version": "12",
                    "Name": "Chef"
                },
                {
                    "Version": "11.10",
                    "Name": "Chef"
                },
                {
                    "Version": "11.4",
                    "Name": "Chef"
                },
                {
                    "Version": "0.9",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "Amazon Linux 2016.09",
            "Type": "Linux",
            "ReportedVersion": "2016.09",
            "ReportedName": "amazon",
            "Id": "Amazon Linux 2016.09",
            "ConfigurationManagers": [
                {
                    "Version": "12",
                    "Name": "Chef"
                },
                {
                    "Version": "11.10",
                    "Name": "Chef"
                },
                {
                    "Version": "11.4",
                    "Name": "Chef"
                },
                {
                    "Version": "0.9",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "Amazon Linux 2017.03",
            "Type": "Linux",
            "ReportedVersion": "2017.03",
            "ReportedName": "amazon",
            "Id": "Amazon Linux 2017.03",
            "ConfigurationManagers": [
                {
                    "Version": "12",
                    "Name": "Chef"
                },
                {
                    "Version": "11.10",
                    "Name": "Chef"
                },
                {
                    "Version": "11.4",
                    "Name": "Chef"
                },
                {
                    "Version": "0.9",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "Amazon Linux 2017.09",
            "Type": "Linux",
            "ReportedVersion": "2017.09",
            "ReportedName": "amazon",
            "Id": "Amazon Linux 2017.09",
            "ConfigurationManagers": [
                {
                    "Version": "12",
                    "Name": "Chef"
                },
                {
                    "Version": "11.10",
                    "Name": "Chef"
                },
                {
                    "Version": "11.4",
                    "Name": "Chef"
                },
                {
                    "Version": "0.9",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "Amazon Linux 2018.03",
            "Type": "Linux",
            "ReportedVersion": "2018.03",
            "ReportedName": "amazon",
            "Id": "Amazon Linux 2018.03",
            "ConfigurationManagers": [
                {
                    "Version": "12",
                    "Name": "Chef"
                },
                {
                    "Version": "11.10",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "CentOS Linux 7",
            "Type": "Linux",
            "ReportedVersion": "7",
            "ReportedName": "CentOS Linux",
            "Id": "CentOS Linux 7",
            "ConfigurationManagers": [
                {
                    "Version": "12",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "Microsoft Windows Server 2012 R2 Base",
            "Type": "Windows",
            "ReportedVersion": "2012 r2 standard",
            "ReportedName": "microsoft windows server",
            "Id": "Microsoft Windows Server 2012 R2 Base",
            "ConfigurationManagers": [
                {
                    "Version": "12.2",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "Microsoft Windows Server 2012 R2 with SQL Server Express",
            "Type": "Windows",
            "ReportedVersion": "2012 r2 standard",
            "ReportedName": "microsoft windows server",
            "Id": "Microsoft Windows Server 2012 R2 with SQL Server Express",
            "ConfigurationManagers": [
                {
                    "Version": "12.2",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "Microsoft Windows Server 2012 R2 with SQL Server Standard",
            "Type": "Windows",
            "ReportedVersion": "2012 r2 standard",
            "ReportedName": "microsoft windows server",
            "Id": "Microsoft Windows Server 2012 R2 with SQL Server Standard",
            "ConfigurationManagers": [
                {
                    "Version": "12.2",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "Microsoft Windows Server 2012 R2 with SQL Server Web",
            "Type": "Windows",
            "ReportedVersion": "2012 r2 standard",
            "ReportedName": "microsoft windows server",
            "Id": "Microsoft Windows Server 2012 R2 with SQL Server Web",
            "ConfigurationManagers": [
                {
                    "Version": "12.2",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "Red Hat Enterprise Linux 7",
            "Type": "Linux",
            "ReportedVersion": "7",
            "ReportedName": "Red Hat Enterprise Linux",
            "Id": "Red Hat Enterprise Linux 7",
            "ConfigurationManagers": [
                {
                    "Version": "12",
                    "Name": "Chef"
                },
                {
                    "Version": "11.10",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "Ubuntu 12.04 LTS",
            "Type": "Linux",
            "Supported": false,
            "ReportedVersion": "12.04",
            "ReportedName": "ubuntu",
            "Id": "Ubuntu 12.04 LTS",
            "ConfigurationManagers": [
                {
                    "Version": "12",
                    "Name": "Chef"
                },
                {
                    "Version": "11.10",
                    "Name": "Chef"
                },
                {
                    "Version": "11.4",
                    "Name": "Chef"
                },
                {
                    "Version": "0.9",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "Ubuntu 14.04 LTS",
            "Type": "Linux",
            "ReportedVersion": "14.04",
            "ReportedName": "ubuntu",
            "Id": "Ubuntu 14.04 LTS",
            "ConfigurationManagers": [
                {
                    "Version": "12",
                    "Name": "Chef"
                },
                {
                    "Version": "11.10",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "Ubuntu 16.04 LTS",
            "Type": "Linux",
            "ReportedVersion": "16.04",
            "ReportedName": "ubuntu",
            "Id": "Ubuntu 16.04 LTS",
            "ConfigurationManagers": [
                {
                    "Version": "12",
                    "Name": "Chef"
                }
            ]
        },
        {
            "Name": "Ubuntu 18.04 LTS",
            "Type": "Linux",
            "ReportedVersion": "18.04",
            "ReportedName": "ubuntu",
            "Id": "Ubuntu 18.04 LTS",
            "ConfigurationManagers": [
                {
                    "Version": "12",
                    "Name": "Chef"
                }
            ]
        },
        {
            "ConfigurationManagers": [
                {
                    "Version": "12",
                    "Name": "Chef"
                },
                {
                    "Version": "11.10",
                    "Name": "Chef"
                },
                {
                    "Version": "11.4",
                    "Name": "Chef"
                },
                {
                    "Version": "0.9",
                    "Name": "Chef"
                }
            ],
            "Type": "Linux",
            "Name": "Custom",
            "Id": "Custom"
        },
        {
            "ConfigurationManagers": [
                {
                    "Version": "12.2",
                    "Name": "Chef"
                }
            ],
            "Type": "Windows",
            "Name": "CustomWindows",
            "Id": "CustomWindows"
        }
    ]
}
```

```


```

**Topics**
+ [Linux Operating Systems](workinginstances-os-linux.md)
+ [Microsoft Windows Server](workinginstances-os-windows.md)
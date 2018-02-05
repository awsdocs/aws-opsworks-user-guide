# AWS OpsWorks Stacks Operating Systems<a name="workinginstances-os"></a>

AWS OpsWorks Stacks supports the 64\-bit versions of several built\-in operating systems, including Amazon and Ubuntu Linux distributions, and Microsoft Windows Server\. Some general notes:

+ A stack's instances can run either Linux or Windows\.

  A stack can have different Linux versions or distributions on different instances, but you cannot mix Linux and Windows instances\.

+ You can use custom AMIs \(Amazon Machine Images\), but they must be based on one of the AWS OpsWorks Stacks\-supported AMIs that are described in topics in this section\. While it might be possible to create or register instances with other operating systems \(such as CentOS 6\.*x*\) that have been created from custom or community\-generated AMIs, these are not officially supported\.

  + [Linux Operating Systems](workinginstances-os-linux.md)

  + [Microsoft Windows Server](workinginstances-os-windows.md)

+ You can start and stop instances manually or have AWS OpsWorks Stacks automatically scale the number of instances\.

  You can use time\-based automatic scaling with any stack; Linux stacks also can use load\-based scaling\.

+ In addition to using AWS OpsWorks Stacks to create Amazon EC2 instances, you can also register instances with a Linux stack that were created outside of AWS OpsWorks Stacks\.

  This includes Amazon EC2 instances and instances running on your own hardware\. However, they must be running one of the supported Linux distributions\. You cannot register Amazon EC2 or on\-premises Windows instances\.

You can use the AWS OpsWorks Stacks [https://docs.aws.amazon.com/opsworks/latest/APIReference/API_DescribeOperatingSystems.html](https://docs.aws.amazon.com/opsworks/latest/APIReference/API_DescribeOperatingSystems.html) API to return a list of supported operating systems and their supported versions of Chef\. The following is an example command, using the AWS CLI\.

```
aws opsworks describe-operating-systems
```

The following is an example response\.

```
[
    {operating_systems:
        {Amazon Linux 2017.09:{
            id:Amazon Linux 2017.09,
            type:Linux,
            reported_name:amazon,
            reported_version:2017.09,
            configuration_managers:[
                {name:Chef,version:12},
                {name:Chef,version:11.10},
                {name:Chef,version:11.4},
                {name:Chef,version:0.9}
                ]
            },
        Amazon Linux 2017.03:{
            id:Amazon Linux 2017.03,
            type:Linux,
            reported_name:amazon,
            reported_version:2017.03,
            configuration_managers:[
                {name:Chef,version:12},
                {name:Chef,version:11.10},
                {name:Chef,version:11.4},
                {name:Chef,version:0.9}
                ]
            },
        Amazon Linux 2016.09:{
            id:Amazon Linux 2016.09,
            type:Linux,
            reported_name:amazon,
            reported_version:2016.09,
            configuration_managers:[
                {name:Chef,version:12},
                {name:Chef,version:11.10},
                {name:Chef,version:11.4},
                {name:Chef,version:0.9}
                ]
            },
        Amazon Linux 2016.03:{
            id:Amazon Linux 2016.03,
            type:Linux,
            reported_name:amazon,
            reported_version:2016.03,
            configuration_managers:[
                {name:Chef,version:12},
                {name:Chef,version:11.10},
                {name:Chef,version:11.4},
                {name:Chef,version:0.9}
                ]
            },
        Amazon Linux 2015.09:{
            id:Amazon Linux 2015.09,
            type:Linux,
            reported_name:amazon,
            reported_version:2015.09,
            configuration_managers:[
                {name:Chef,version:12},
                {name:Chef,version:11.10},
                {name:Chef,version:11.4},
                {name:Chef,version:0.9}
                ],
            unsupported:true
            },
        Amazon Linux 2015.03:{
            id:Amazon Linux 2015.03,
            type:Linux,
            reported_name:amazon,
            reported_version:2015.03,
            configuration_managers:[
                {name:Chef,version:12},
                {name:Chef,version:11.10},
                {name:Chef,version:11.4},
                {name:Chef,version:0.9}
                ],
            unsupported:true
            },
        Amazon Linux 2014.09:{
            id:Amazon Linux 2014.09,
            reported_name:amazon,
            reported_version:2014.09,
            type:Linux,
            configuration_managers:[
                {name:Chef,version:11.10},
                {name:Chef,version:11.4},
                {name:Chef,version:0.9}
                ],
            unsupported:true
            },
        Amazon Linux 2014.03:{
            id:Amazon Linux,
            reported_name:amazon,
            reported_version:2014.03,
            type:Linux,
            configuration_managers:[
                {name:Chef,version:11.10},
                {name:Chef,version:11.4},
                {name:Chef,version:0.9}
                ],
            unsupported:true
            },
        CentOS Linux 7:{
            id:CentOS Linux 7,
            reported_name:CentOS Linux,
            reported_version:7,
            type:Linux,
            configuration_managers:[
                {name:Chef,version:12}
                ]
            },
        Red Hat Enterprise Linux 7:{
            id:Red Hat Enterprise Linux 7,
            reported_name:Red Hat Enterprise Linux,
            reported_version:7,
            type:Linux,
            configuration_managers:[
                {name:Chef,version:12},
                {name:Chef,version:11.10}
                ]
            },
        Ubuntu 16.04 LTS:{
            id:Ubuntu 16.04 LTS,
            reported_name:ubuntu,
            reported_version:16.04,
            type:Linux,
            configuration_managers:[
                {name:Chef,version:12}
                ]
            },
        Ubuntu 14.04 LTS:{
            id:Ubuntu 14.04 LTS,
            reported_name:ubuntu,
            reported_version:14.04,
            type:Linux,
            configuration_managers:[
                {name:Chef,version:12},
                {name:Chef,version:11.10}
                ]
            },
        Ubuntu 12.04 LTS:{
            id:Ubuntu 12.04 LTS,
            reported_name:ubuntu,
            reported_version:12.04,
            type:Linux,
            configuration_managers:[
                {name:Chef,version:12},
                {name:Chef,version:11.10},
                {name:Chef,version:11.4},
                {name:Chef,version:0.9}
                ],
            unsupported:true
            },
        Use custom Linux AMI:{
            id:Custom,
            type:Linux,
            configuration_managers:[
                {name:Chef,version:12},
                {name:Chef,version:11.10},
                {name:Chef,version:11.4},
                {name:Chef,version:0.9}
                ]
            },
        Microsoft Windows Server 2012 R2 Base:{
            id:Microsoft Windows Server 2012 R2 Base,
            reported_name:microsoft windows server,
            reported_version:2012 r2 standard,
            type:Windows,
            configuration_managers:[
                {name:Chef,version:12.2}
                ]
            },
        Microsoft Windows Server 2012 R2 with SQL Server Express:{
            id:Microsoft Windows Server 2012 R2 with SQL Server Express,
            reported_name:microsoft windows server,
            reported_version:2012 r2 standard,
            type:Windows,
            configuration_managers:[
                {name:Chef,version:12.2}
                ]
            },
        Microsoft Windows Server 2012 R2 with SQL Server Standard:{
            id:Microsoft Windows Server 2012 R2 with SQL Server Standard,
            reported_name:microsoft windows server,
            reported_version:2012 r2 standard,
            type:Windows,
            configuration_managers:[
                {name:Chef,version:12.2}
                ]
            },
        Microsoft Windows Server 2012 R2 with SQL Server Web:{
            id:Microsoft Windows Server 2012 R2 with SQL Server Web,
            reported_name:microsoft windows server,
            reported_version:2012 r2 standard,
            type:Windows,
            configuration_managers:[
                {name:Chef,version:12.2}
                ]
            },
        Use custom Windows AMI:{
            id:CustomWindows,
            type:Windows,
            configuration_managers:[
                {name:Chef,version:12.2}
                ]
            }
        }
    }
],
RequestId:d000f31a-fc00-00e0-a0004-000ee2b4c5ce,
timestamp:2018-01-18T16:01:31.545812+00:00,
LogLevel:INFO,
Program:opsworks-stacks,PID:10090
}
```

```


```


+ [Linux Operating Systems](workinginstances-os-linux.md)
+ [Microsoft Windows Server](workinginstances-os-windows.md)
# Ruby Versions<a name="workingcookbook-ruby"></a>

All instances in a Linux stack have Ruby installed\. AWS OpsWorks Stacks installs a Ruby package on each instance, which it uses to run Chef recipes and the instance agent\. AWS OpsWorks Stacks determines the Ruby version based on which Chef version the stack is running\. Do not attempt to modify this version; doing so might disable the instance agent\.

AWS OpsWorks Stacks does not install an application Ruby executable on Windows stacks\. The Chef 12\.2 client comes with Ruby 2\.0\.0 p451, but the Ruby executable is not added to the instances' PATH environment variable\. If you want to use this executable to run Ruby code, it is located at `\opscode\chef\embedded\bin\ruby.exe` on your Windows drive\.

The following table summarizes AWS OpsWorks Stacks Ruby versions\. The available application Ruby versions also depend on the instance's operating system\. For more information, including the available patch versions, see [AWS OpsWorks Stacks Operating Systems](workinginstances-os.md)\.


| Chef Version | Chef Ruby Version | Available Application Ruby Versions | 
| --- | --- | --- | 
| 0\.9 \(c\) | 1\.8\.7 | 1\.8\.7\(a\), 1\.9\.3\(e\), 2\.0\.0 | 
| 11\.4 \(c\) | 1\.8\.7 | 1\.8\.7\(a\), 1\.9\.3\(e\), 2\.0\.0, 2\.1, 2\.2\.0, 2\.3 | 
| 11\.10 | 2\.0\.0\-p481 | 1\.9\.3\(c, e\), 2\.0\.0, 2\.1, 2\.2\.0, 2\.3, 2\.6\.1 | 
| 12 \(b\) | 2\.1\.6, 2\.2\.3 | None | 
| 12\.22 \(d\) | 2\.3\.6 | None | 

**\(a\)** Not available with Amazon Linux 2014\.09 and later, Red Hat Enterprise Linux \(RHEL\), or Ubuntu 14\.04 LTS\.

**\(b\)** Available only on Linux stacks\.

**\(c\)** Not available with RHEL\.

**\(d\)** Available only on Windows stacks\. Major version is 12\.2\. Current minor version is 12\.22\.

**\(e\)** Deprecation is complete; support has ended\.

The install locations depend on the Chef version:
+ Applications use the `/usr/local/bin/ruby` executable for all Chef Versions\.
+ For Chef 0\.9 and 11\.4, the instance agent and Chef recipes use the `/usr/bin/ruby` executable\.
+ For Chef 11\.10, the instance agent and Chef recipes use the `/opt/aws/opsworks/local/bin/ruby` executable\. 
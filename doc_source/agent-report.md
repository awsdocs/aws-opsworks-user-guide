# agent\_report<a name="agent-report"></a>

Returns an agent report\.

```
sudo opsworks-agent-cli agent_report
```

The following output example is from an instance that most recently ran a configure activity\.

```
$ sudo opsworks-agent-cli agent_report

AWS OpsWorks Instance Agent State Report:

  Last activity was a "configure" on 2015-12-01 18:19:23 UTC
  Agent Status: The AWS OpsWorks agent is running as PID 30998
  Agent Version: 4004-20151201152533, up to date
```
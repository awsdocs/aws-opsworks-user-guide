# List a Stack's Apps \(describe\-apps\)<a name="cli-examples-describe-apps"></a>

Use the [describe\-apps](http://docs.aws.amazon.com/cli/latest/reference/opsworks/describe-apps.html) command to list a stack's apps or get details about specified apps\.

```
aws opsworks --region us-west-1 describe-apps --stack-id 38ee91e2-abdc-4208-a107-0b7168b3cc7a
```

The preceding example returns a JSON object that contains information about each app\. This example has only one app\. For a description of each parameter, see [describe\-apps](http://docs.aws.amazon.com/cli/latest/reference/opsworks/describe-apps.html)\.

```
{
  "Apps": [
    {
      "StackId": "38ee91e2-abdc-4208-a107-0b7168b3cc7a",
      "AppSource": {
        "Url": "https://s3-us-west-2.amazonaws.com/opsworks-tomcat/simplejsp.zip",
        "Type": "archive"
      },
      "Name": "SimpleJSP",
      "EnableSsl": false,
      "SslConfiguration": {},
      "AppId": "da1decc1-0dff-43ea-ad7c-bb667cd87c8b",
      "Attributes": {
        "RailsEnv": null,
        "AutoBundleOnDeploy": "true",
        "DocumentRoot": "ROOT"
      },
      "Shortname": "simplejsp",
      "Type": "other",
      "CreatedAt": "2013-08-01T21:46:54+00:00"
    }
  ]
}
```
#### Platform API Request Step Scope

Platform API Request steps add the step execution values described in the example JSON below to the formula context. The formula context is then passed from step-to-step, allowing you to use these values in any subsequent steps in your formula.

```json
{
  "myPlatformStep": {
    "request": {
        "query": "{}",
        "body": "{\"Name\":\"New Account Name\"}",
        "method": "POST",
        "path": "{}",
        "uri": "/elements/api-v2/hubs/crm/accounts",
        "headers": "{\"authorization\":\"Element /ABC=, User DEF=, Organization GHI\",\"content-length\":\"14\",\"host\":\"jjwyse.ngrok.io\",\"content-type\":\"application/json}"
    },
    "response": {
      "code": "200",
      "headers": "{\"Set-Cookie\": \"CESESSIONID=2CA15552EE56EAF65BF1102F6CACEACC;Path=/elements/;HttpOnly\"}",
      "body": "{\"Id\": \"001tx3WcAAI\", \"Name\": \"New Account Name\"}"
    }
  }
}
```
Example references to Platform API Request scope:

* `${steps.myPlatformStep.request}`
* `${steps.myPlatformStep.request.body}`
* `${steps.myPlatformStep.response.code}`

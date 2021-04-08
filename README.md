
# Missing product in output ARM template

When I run 

```
apim-templates create --configFile config.yaml
```
to create ARM templates for an API that is associated with a Product, 
I expect the output template to contain that product. E.g.

What I expect: 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "ApimServiceName": {
      "type": "string"
    }
  },
  "resources": [
    {
      "properties": {
        "type": "http",
        "isCurrent": false,
        "subscriptionRequired": false,
        "displayName": "myAPI",
        "path": "my-api",
        "protocols": [
          "https"
        ],
        "value": "openapi: \"3.0.0\"\ninfo:\n  title: myAPI\n  version: \"1.0\"\nservers:\n  - url: http://localhost:8000\npaths:\n  /:\n    get:\n      summary: myAPI\n      description: This is myAPI\n      operationId: hello.post_greeting\n      responses:\n        200:\n          description: the response\n          content:\n            text/plain:\n              schema:\n                type: string\n                example: \"hello from myAPI!\"\n",
        "format": "openapi",
        "apiType": "http"
      },
      "name": "[concat(parameters('ApimServiceName'), '/myAPI')]",
      "type": "Microsoft.ApiManagement/service/apis",
      "apiVersion": "2019-01-01",
      "dependsOn": []
    },
    {
      "properties": {},
      "name": "[concat(parameters('ApimServiceName'), '/myProduct/myAPI')]",
      "type": "Microsoft.ApiManagement/service/products/apis",
      "apiVersion": "2019-01-01",
      "dependsOn": [
        "[resourceId('Microsoft.ApiManagement/service/apis', parameters('ApimServiceName'), 'myAPI')]"
      ]
    }
  ]
}
```

What I get: 


```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "ApimServiceName": {
      "type": "string"
    },
    "serviceUrl": {
      "type": "object"
    }
  },
  "resources": [
    {
      "properties": {
        "type": "http",
        "isCurrent": false,
        "subscriptionRequired": false,
        "displayName": "myAPI",
        "serviceUrl": "[parameters('serviceUrl').myAPI]",
        "path": "my-api",
        "protocols": [
          "https"
        ],
        "value": "openapi: \"3.0.0\"\ninfo:\n  title: myAPI\n  version: \"1.0\"\nservers:\n  - url: http://localhost:8000\npaths:\n  /:\n    get:\n      summary: myAPI\n      description: This is myAPI\n      operationId: hello.post_greeting\n      responses:\n        200:\n          description: the response\n          content:\n            text/plain:\n              schema:\n                type: string\n                example: \"hello from myAPI!\"\n",
        "format": "openapi",
        "apiType": "http"
      },
      "name": "[concat(parameters('ApimServiceName'), '/myAPI')]",
      "type": "Microsoft.ApiManagement/service/apis",
      "apiVersion": "2019-01-01",
      "dependsOn": []
    }
  ]
}
 ```

 If I revert the `azure-api-management-devops-resource-kit` to `7c4bff76cc0e0f99f835a4ae27e56c3474fd3907` the output is what I expect. If I however use the latest from the master branch, the output is missing the product. 


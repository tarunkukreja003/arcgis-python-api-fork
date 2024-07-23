# docker

Some useful docker images for utilizing the ArcGIS API for Python in your workflows

## LambdaBaseImage

#### ghcr.io/esri/arcgis-python-api-lambda:latest

To use this image, setup your dockerfile like:
```
FROM ghcr.io/esri/arcgis-python-api-lambda:latest
COPY app.py ${LAMBDA_TASK_ROOT}
```

your app.py should have a handler method:
```
import arcgis

def handler(event, context):
    """
    AWS Lambda Handler
    """
    print(f"Hello from AWS Lambda using ArcGIS API for Python {arcgis.__version__}!")
```

Push to your _private_ AWS ECR instance, and configure lambda to run from this container image.  As of this writing, public AWS ECR instances are not supported for lambda.

## AzureFunctionsBaseImage

#### ghcr.io/esri/arcgis-python-api-azure-functions:latest

To use this image, setup your dockerfile like
```
FROM ghcr.io/esri/arcgis-python-api-azure-functions:latest
COPY . /home/site/wwwroot
```

Your copied resources will need to include:
- `function.json`, such as https://github.com/Azure/azure-functions-docker-python-sample/blob/dev/HttpTrigger/function.json
- `main.py`, such as:

```
import arcgis
import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:
    return func.HttpResponse(f"Hello from Azure Functions using ArcGIS API for Python {arcgis.__version__}!")
```

For futher information, see:
- https://github.com/Azure/azure-functions-python-worker
- https://github.com/Azure/azure-functions-docker-python-sample
- https://learn.microsoft.com/en-us/azure/azure-functions/functions-how-to-custom-container
# Django Rest API with Documentation using Swagger

A simple API written using the Django Rest Framework and documented using Swagger

## Overview:

* We need to create a schema. The schema outlines all the endpoints and actions of our API.

* We need to create the documentation that is a more human-readable form of the schema.


## Steps:

### Generate schema

1. install `pyyaml` and `uritemplate`
2. edit the top level `urls.py` file to include:

```Python
from rest_framework.schemas import get_schema_view

urlpatterns = [
    # ...
    path('api_schema/', get_schema_view(
        title='API Schema',
        description='Guide for the REST API'
    ), name='api_schema'),
    # ...
]
```

The schema will now be found at `http://http//127.0.0.1:8000/api_schema`

### Generate documentation

1. install Django Swagger module:

`pip install django-rest-swagger`

2. add it to to **INSTALLED_APPS** and update the **TEMPLATES** section

3. create the HTML template file eg. `docs.html` in **templates/**

```HTML
<!DOCTYPE html>
<html>
  <head>
    <title>School Service Documentation</title>
    <meta charset="utf-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" type="text/css" href="//unpkg.com/swagger-ui-dist@3/swagger-ui.css" />
  </head>
  <body>
    <div id="swagger-ui"></div>
    <script src="//unpkg.com/swagger-ui-dist@3/swagger-ui-bundle.js"></script>
    <script>
    const ui = SwaggerUIBundle({
        url: "{% url schema_url %}",
        dom_id: '#swagger-ui',
        presets: [
          SwaggerUIBundle.presets.apis,
          SwaggerUIBundle.SwaggerUIStandalonePreset
        ],
        layout: "BaseLayout"
      })
    </script>
  </body>
</html>

```

4. update `urls.py` to include swagger doc:

```Python
from django.views.generic import TemplateView

urlpatterns = [
    path('docs/', TemplateView.as_view(
        template_name='docs.html',
        extra_context={'schema_url':'api_schema'}
        ), name='swagger-ui'),
]
```

API docs will now be found at `http://127.0.0.1:8000/swagger-ui`
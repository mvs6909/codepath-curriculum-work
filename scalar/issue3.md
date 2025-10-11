## Metadata (not to include in the student issue)

PR Link - https://github.com/scalar/scalar/pull/6653
Issue link - https://github.com/scalar/scalar/issues/6476
Tool - https://openbootstrap.onrender.com/pr/scalar/scalar/6653

**Notes**: This issue's PR description is a bit different than our forked commit. CodePath's repo does not have any default sorting to `required an alpha`. Still, this is a great issue to work on helping students touch multiple parts of the api-reference code. I have updated the PR description accordingly.

# Add configuration options for sorting schema properties

## Motivation

When rendering OpenAPI schema definitions, the order in which properties are displayed can significantly impact the developer experience. Different use cases call for different ordering strategies: some users prefer alphabetical sorting for easier scanning, others want required properties highlighted at the top, and some need to preserve the exact order from their OpenAPI specification to emphasize important fields or maintain logical groupings.

## Current Behavior

Currently, the API reference preserves the property order as defined in the OpenAPI specification, with no sorting or grouping options available. Users cannot choose to sort properties alphabetically, group required properties first, or otherwise customize the display order. This feature will introduce configuration options to allow users to control how schema properties are sorted in the rendered documentation.

**Reproduction Steps:**

1. Create an OpenAPI specification at `package/api-reference/openapi.json` with a schema that has properties defined in a specific order (e.g., `zebra`, `alpha`, `beta`, `gamma`)
Example OpenAPI schema for testing property order:

```json
{
  "openapi": "3.0.1",
  "info": {
    "title": "Test API",
    "version": "1.0.0",
    "description": "API with test endpoint"
  },
  "paths": {
    "/Test": {
      "post": {
        "summary": "Test",
        "operationId": "Test",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/TestRequest"
              }
            }
          },
          "required": true
        },
        "responses": {
          "204": {
            "description": "No Content"
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "TestRequest": {
        "required": [
          "zebra",
          "gamma"
        ],
        "type": "object",
        "properties": {
          "zebra": {
            "maxLength": 50,
            "type": "string"
          },
          "beta": {
            "maxLength": 50,
            "type": "string"
          },
          "alpha": {
            "maxLength": 50,
            "type": "string"
          },
          "gamma": {
            "maxLength": 50,
            "type": "string"
          }
        }
      }
    }
  }
}
```
2. Observe some properties as required (e.g., `zebra` and `gamma`)
3. Render the schema in the API reference by adding creating a file in `packages/api-reference/` and add the file to the `index.html`
```
          {
            title: 'Bolt',
            url: 'https://assets.bolt.com/external-api-references/bolt.yml',
          },
          {
            title: 'OpenStatus',
            url: 'https://api.openstatus.dev/v1/openapi',
          },
+          {
+            title: 'Reproducing Issue',
+            url: 'openapi.json',
+          },
        ],
        persistAuth: true,
        // Avoid CORS issues
        proxyUrl: 'https://proxy.scalar.com',
      })
```
4. Run `pnpm dev:reference` and open the OpenAPI Spec created by selecting the title: `Reproducing Issue`
5. Observe the ordering is preserved: `zebra, beta, alpha, gamma` and no sorting options are available
6. Note: Currently, there is no configuration option to sort parameters (such as those in the request or response) by required status or alphabetically. The sorting options described above apply only to schema properties, not to parameters.

## Expected Behavior

Users should be able to configure how schema properties are sorted through two independent configuration options that work together to control the final display order. They should be able to provide configuration values to set the sorting preferences by introducing `orderSchemaPropertiesBy` and `orderRequiredPropertiesFirst`
| `orderSchemaPropertiesBy` | `orderRequiredPropertiesFirst` | Display Order of Properties                                                                                 | Example Output (`zebra`, `beta`, `alpha`, `gamma`; required: `zebra`, `gamma`) |
|---------------------------|-------------------------------|------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------|
| `'alpha'` (default)       | `true` (default)              | Required properties (alphabetically), then optional properties (alphabetically)                            | `gamma`, `zebra`, `alpha`, `beta`                                              |
| `'alpha'`                 | `false`                       | All properties sorted alphabetically, regardless of required status                                        | `alpha`, `beta`, `gamma`, `zebra`                                              |
| `'preserve'`              | `true`                        | Required properties (original order), then optional properties (original order)                            | `zebra`, `gamma`, `beta`, `alpha`                                              |
| `'preserve'`              | `false`                       | All properties in their original order from the OpenAPI specification                                      | `zebra`, `beta`, `alpha`, `gamma`                                              |

**Acceptance Criteria:**

- [ ] The `orderSchemaPropertiesBy` configuration option accepts `'alpha'` (default) or `'preserve'` to control alphabetical sorting vs preserving original order
- [ ] The `orderRequiredPropertiesFirst` boolean configuration option (default `true`) controls whether required properties are grouped before optional properties
- [ ] All combinations of these configuration options must result in the expected property display order as described in the table above
- [ ] The configuration options are properly typed and validated with appropriate defaults
- [ ] Unit tests are implemented to verify all combinations of `orderSchemaPropertiesBy` and `orderRequiredPropertiesFirst` produce the expected property order
- Update the `documentation/configuration.md` with the new configuration options available.
## Verification

**Manual Testing:**

1. Use the spec created during Reproducing the issue and update the `packages/api-reference/index.html` to include the new properties.
2. Set the `orderSchemaPropertiesBy` and `orderRequiredPropertiesFirst` for all combinations and match the output.

(example `package/api-reference/index.html`) to test out all combinations of sorting
```json
<!doctype html>
<html>
  <head>
    <title>Scalar API Reference</title>
    <meta charset="utf-8" />
    <meta
      name="viewport"
      content="width=device-width, initial-scale=1" />
  </head>

  <body>
    <div id="app"></div>

    <!-- Our Script -->
    <script
      src="src/standalone.ts"
      type="module"></script>

    <!-- Initialize the Scalar API Reference -->
    <script type="module">
      Scalar.createApiReference('#app', {
        sources: [
          {
            title: 'Alpha + RequiredFirst: true',
            url: 'issue3openapi.json',
            orderRequiredPropertiesFirst: true,
            orderSchemaPropertiesBy: 'alpha',
          },
          {
            title: 'Alpha + RequiredFirst: false',
            url: 'issue3openapi.json',
            orderRequiredPropertiesFirst: false,
            orderSchemaPropertiesBy: 'alpha',
          },
          {
            title: 'Preserve + RequiredFirst: true',
            url: 'issue3openapi.json',
            orderRequiredPropertiesFirst: true,
            orderSchemaPropertiesBy: 'preserve',
          },
          {
            title: 'Preserve + false',
            url: 'issue3openapi.json',
            orderRequiredPropertiesFirst: false,
            orderSchemaPropertiesBy: 'preserve',
          },
        ],
        persistAuth: true,
        // Avoid CORS issues
        proxyUrl: 'https://proxy.scalar.com',
      })
    </script>
  </body>
</html>

```

**Automated Testing:**

Run the test suite to verify the sorting logic handles all configuration combinations correctly. Tests should cover scenarios with various property orders, required/optional mixes, and all configuration option combinations.

### Submission
Download https://cap.so/ to record your screen (use Studio mode). Export as an mp4, and drag and drop into an issue comment below.

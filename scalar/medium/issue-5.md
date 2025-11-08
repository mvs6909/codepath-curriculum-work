# Request body properties not sorted according to configuration options

## Motivation

The API reference component provides configuration options (`orderSchemaPropertiesBy` and `orderRequiredPropertiesFirst`) to control how schema properties are displayed to users. These options are critical for API documentation usability - developers need to quickly find required fields, and some teams prefer alphabetical ordering while others want to preserve the original specification order.

Currently, these sorting options work correctly for schema object properties displayed in the main view, but they are not applied to request body properties before the UI splits them into "visible" and "additional" (collapsed) sections. This causes inconsistent behavior where required properties may appear after optional properties, or properties don't follow the configured sort order when some are hidden under "show additional properties". This breaks the user experience and makes the configuration options unreliable.

## Current Behavior

When viewing API endpoints with request bodies containing many properties, the sorting configuration options (`orderSchemaPropertiesBy` and `orderRequiredPropertiesFirst`) are not consistently applied. Properties are split into visible and collapsed sections before sorting is applied, resulting in:

- Required properties appearing after optional properties in the collapsed section
- Properties not following alphabetical order when `orderSchemaPropertiesBy: 'alpha'` is set
- Original specification order not preserved when `orderSchemaPropertiesBy: 'preserve'` is set
- Inconsistent ordering between the visible properties and the "additional properties" section

**Reproduction Steps:**

1. Configure the API reference with `orderRequiredPropertiesFirst: true` and `orderSchemaPropertiesBy: 'alpha'`
2. Load an OpenAPI specification with an endpoint that has a request body containing more than 10 properties (some required, some optional)
3. Navigate to that endpoint in the API reference
4. Click "show additional properties" to expand the collapsed properties section
5. Observe: Required properties may appear in the collapsed section after optional properties in the visible section, and properties are not sorted alphabetically across both sections

## Expected Behavior

Request body properties should be sorted according to the configured options (`orderSchemaPropertiesBy` and `orderRequiredPropertiesFirst`) before being partitioned into visible and collapsed sections. This ensures consistent ordering throughout the entire property list.

**Acceptance Criteria:**

- [ ] Request body properties are sorted before being split into visible and additional properties sections
- [ ] When `orderRequiredPropertiesFirst: true`, all required properties appear before optional properties across both visible and collapsed sections
- [ ] When `orderSchemaPropertiesBy: 'alpha'`, properties are sorted alphabetically across the entire list
- [ ] When `orderSchemaPropertiesBy: 'preserve'`, the original specification order is maintained
- [ ] The sorting logic is reusable and can be applied consistently across different components that display schema properties

## Verification

**Manual Testing:**
1. Create or use an OpenAPI specification with a request body containing 15+ properties (mix of required and optional)
2. Configure the API reference with `orderRequiredPropertiesFirst: true` and `orderSchemaPropertiesBy: 'alpha'`
3. Load the specification and navigate to the endpoint
4. Verify that all required properties appear first, sorted alphabetically
5. Click "show additional properties" and verify the collapsed properties continue the alphabetical order with optional properties
6. Change configuration to `orderRequiredPropertiesFirst: false` and verify properties are sorted alphabetically regardless of required status
7. Change configuration to `orderSchemaPropertiesBy: 'preserve'` and verify the original specification order is maintained

**Automated Testing:**
- Run the test suite to verify sorting behavior with various configurations
- Tests should cover: alphabetical sorting, required-first sorting, discriminator properties, filtering read-only/write-only properties, and edge cases with empty or missing properties
- Execute: `pnpm test` (or the appropriate test command for the repository)
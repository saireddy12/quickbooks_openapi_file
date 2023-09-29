# QuickBooks Online Accounting API OpenAPI 3.0.0 Specification

## Problem Statement

The QuickBooks Online Accounting API provides a wealth of functionalities for financial data, but its OpenAPI 3.0.0 specification comes with certain limitations. One of the main issues is the inability to handle duplicate methods within the same API paths, leading to inconsistencies and potential loss of information when converted from other formats like Postman collections.

## Solution

To address these issues, the following steps were taken:

### Handling Duplicate Methods and Paths

To make each path-method combination unique, an extra string (`dupe_extra_path_n`, where `n` is an integer) was appended to the duplicate paths. This ensures that each API endpoint is represented distinctly in the OpenAPI file.

For example:
- `/v1/users` might become `/v1/users/dupe_extra_path_1`
- `/v1/users` might become `/v1/users/dupe_extra_path_2`

### Python Script for Cleanup

A Python script was written to read this customized OpenAPI file, remove the appended extra strings, and store the path data in a list format. This makes it easier for further processing or usage within API documentation tools.

Here's the Python function used for this purpose:

```python
def remove_dupe_extra_path(url):
    """
    Removes the 'dupe_extra_path_' part from the URL and returns the rest of it.
    Example:
    >>> remove_dupe_extra_path("v1/users/dupe_extra_path_1") -> 'v1/users'
    """
    try:
        modified_url = re.sub(r'/dupe_extra_path_\d+', '', url)
    except Exception as e:
        logging.error(f"Error while removing dupe_extra_path from url: {e}", exc_info=True)
        return url
    return modified_url
```


### Steps Taken for OpenAPI File Generation

1. **Converted the Postman Collection**: The first step involved converting the existing Postman collection into an OpenAPI specification. The original Postman collection can be accessed from [this link](https://elements.getpostman.com/view/import?collection=c4c9e1618576a68ac526&&referrer=https%3A%2F%2Fdeveloper.intuit.com%2Fapp%2Fdeveloper%2Fqbo%2Fdocs%2Fdevelop%2Fsandboxes%2Fpostman#?env%5BQBOV3-OAuth2-Env-Variables%5D=W3sidHlwZSI6InRleHQiLCJlbmFibGVkIjp0cnVlLCJrZXkiOiJiYXNldXJsIiwidmFsdWUiOiJzYW5kYm94LXF1aWNrYm9va3MuYXBpLmludHVpdC5jb20ifSx7InR5cGUiOiJ0ZXh0IiwiZW5hYmxlZCI6dHJ1ZSwia2V5IjoiY29tcGFueWlkIiwidmFsdWUiOiI8PGNvbXBhbnlJZC9yZWFsbUlkPj4ifSx7InR5cGUiOiJ0ZXh0IiwiZW5hYmxlZCI6dHJ1ZSwia2V5IjoibWlub3J2ZXJzaW9uIiwidmFsdWUiOiIxNCJ9LHsidHlwZSI6InRleHQiLCJlbmFibGVkIjp0cnVlLCJrZXkiOiJVc2VyQWdlbnQiLCJ2YWx1ZSI6IlFCT1YzLU9BdXRoMi1Qb3N0bWFuLUNvbGxlY3Rpb24ifV0=&env%5BQBOV3-Payments-Env-OAuth2%5D=W3sidHlwZSI6InRleHQiLCJlbmFibGVkIjp0cnVlLCJrZXkiOiJjbGllbnRJZCIsInZhbHVlIjoiPDxFbnRlciBDbGllbnRJZCBmcm9tIGFwcGNlbnRlcj4+In0seyJ0eXBlIjoidGV4dCIsImVuYWJsZWQiOnRydWUsImtleSI6ImNsaWVudFNlY3JldCIsInZhbHVlIjoiPDxFbnRlciBDbGllbnRTZWNyZXQgZnJvbSBhcHBjZW50ZXI+PiJ9LHsidHlwZSI6InRleHQiLCJlbmFibGVkIjp0cnVlLCJrZXkiOiJiYXNldXJsIiwidmFsdWUiOiJzYW5kYm94LXF1aWNrYm9va3MuYXBpLmludHVpdC5jb20ifSx7InR5cGUiOiJ0ZXh0IiwiZW5hYmxlZCI6dHJ1ZSwia2V5IjoiY29tcGFueWlkIiwidmFsdWUiOiI8PEVudGVyIFJlYWxtSWQgZnJvbSBhcHBjZW50ZXI+PiJ9LHsidHlwZSI6InRleHQiLCJlbmFibGVkIjp0cnVlLCJrZXkiOiJtaW5vcnZlcnNpb24iLCJ2YWx1ZSI6IjEyIn0seyJ0eXBlIjoidGV4dCIsImVuYWJsZWQiOnRydWUsImtleSI6ImJhc2V1cmxwYXltZW50cyIsInZhbHVlIjoic2FuZGJveC5hcGkuaW50dWl0LmNvbSJ9LHsidHlwZSI6InRleHQiLCJlbmFibGVkIjp0cnVlLCJrZXkiOiJVc2VyQWdlbnQiLCJ2YWx1ZSI6IlBheW1lbnRzQVBJLU9BdXRoMi1Qb3N0bWFuIiwiZGVzY3JpcHRpb24iOiIifV0=)

2. **Added `company_id` as Path Parameter**: To make the API more versatile and tailored to specific needs, `company_id` was added as a path parameter to each of the endpoints.

3. **ID to Path Parameter Conversion**: Initially, the paths contained ready IDs. These were manually replaced with variable names wrapped in curly braces, turning them into path parameters. For example, an ID in `/account/123` was replaced with `{account_id}`, resulting in `/account/{account_id}`.

For more details on the API endpoints and their specifications, you can refer to the official [QuickBooks API documentation](https://developer.intuit.com/app/developer/qbo/docs/get-started)


---

This repository contains the modified OpenAPI 3.0.0 specification for QuickBooks Online Accounting API, which addresses the problem of handling duplicate methods and paths in the API. Feel free to use or adapt this solution for your projects.


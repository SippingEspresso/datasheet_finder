# Mouser Search API Reference (v1)

This document provides the technical details for using the Mouser Search API to find component datasheets.

## Base URL
`https://api.mouser.com/api/v1`

## Authentication
The API requires an API key passed as a query parameter.
*   **Parameter Name**: `apiKey`
*   **Format**: UUID string

## Search by Part Number
Finds up to 50 parts matching the provided part number(s).

### Endpoint
`POST /api/v1/search/partnumber`

### Request Parameters
*   **`apiKey`** (Query, Required): Your Mouser API key.
*   **`version`** (Path, Required): The API version (e.g., `v1`).

### Request Body (JSON)
The body should follow this structure:
```json
{
  "SearchByPartRequest": {
    "mouserPartNumber": "PART_NUMBER_HERE",
    "partSearchOptions": "None"
  }
}
```
*   `mouserPartNumber`: String (minimum 3 characters). You can search multiple parts by separating them with a pipe `|`.
*   `partSearchOptions`: Optional. Values: `None` or `Exact`.

### Response Structure (JSON)
The API returns a `SearchResponseRoot` object.
*   **Datasheet URL Location**: `SearchResults` -> `Parts` (Array) -> `DataSheetUrl` (String)

#### Example Response Mapping
```json
{
  "SearchResults": {
    "Parts": [
      {
        "MouserPartNumber": "...",
        "Manufacturer": "...",
        "DataSheetUrl": "https://www.mouser.com/datasheet/...",
        "Description": "..."
      }
    ]
  }
}
```

## Error Handling
Errors are returned in the `Errors` array at the root of the response.
*   Check `Errors` for any messages if `SearchResults` is missing or empty.

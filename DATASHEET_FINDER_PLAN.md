# Datasheet Finder Implementation Plan

## Objective
Create a Python script (`find_datasheet.py`) in the `datasheet_finder` repository. This script will accept a semiconductor IC part number as input and use the Mouser Search API to retrieve and display the URL for its datasheet. It is designed to be executed by the local Hermes agent.

This plan document will also be saved to the repository as `DATASHEET_FINDER_PLAN.md`.

## Requirements
*   **Execution Environment**: Local Hermes agent (Gemma 4 26B).
*   **Python 3.x**: Standard environment.
*   **`requests` library**: For making HTTP REST API calls to Mouser.
*   **`python-dotenv` library**: For securely loading the Mouser API key from a `.env` file.
*   **Mouser API Key**: Required for authentication (user must provide this in the `.env` file).

## Implementation Steps

### 1. Project Setup
*   Navigate to `/home/rings48/datasheet_finder`.
*   Create a `requirements.txt` file containing `requests` and `python-dotenv`.
*   Create a `.env.example` file to demonstrate how the API key should be stored (e.g., `MOUSER_API_KEY=your_key_here`).
*   Save a copy of this plan to `/home/rings48/datasheet_finder/DATASHEET_FINDER_PLAN.md`.

### 2. Script Development (`find_datasheet.py`)
*   **Argument Parsing**: Use the built-in `argparse` module to accept the part number as a command-line argument.
*   **Configuration Management**: Use `dotenv` to load the `MOUSER_API_KEY` from the local environment. Exit gracefully if the key is missing.
*   **API Integration**:
    *   Target the Mouser Search API endpoint (typically `https://api.mouser.com/api/v1/search/partnumber`).
    *   Construct the JSON payload for the search request, specifically querying for the provided part number.
    *   Handle authentication by passing the API key in the query parameters or headers as required by Mouser's specific implementation (usually `apiKey` query param).
*   **Data Parsing**:
    *   Parse the JSON response from Mouser.
    *   Navigate the response structure (typically `SearchResults` -> `Parts` -> `DataSheetUrl`).
    *   Handle edge cases:
        *   Part not found.
        *   Part found, but no datasheet URL available.
        *   API rate limits or authentication errors.
*   **Output**: Print the part number, manufacturer (if available), and the datasheet URL cleanly to the console, formatted in a way that is easily parsable by the Hermes agent if needed.

### 3. Verification & Testing
*   **Initial Smoke Test**: Test the script manually with a known common part number (e.g., `NE555`, `ATMEGA328P`).
*   **Comprehensive Validation**: Use the `IC_TEST_LIST.md` file located in the repository root for extensive testing.
    *   The agent should practice by selecting parts from various manufacturers in the list to ensure the script handles different naming conventions and manufacturer responses correctly.
    *   The goal is to achieve a high success rate in retrieving datasheet URLs for the 125 parts listed.
*   **Error Handling**: Verify that missing environment variables or invalid part numbers are handled gracefully without crashing.

## Future Considerations (Not in initial scope)
*   Adding an option to automatically download the PDF instead of just returning the URL.
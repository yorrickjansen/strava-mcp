# Strava MCP Server

A Model Context Protocol (MCP) server for interacting with the Strava API.

## Features

This MCP server provides tools to access data from the Strava API:

- Get user's activities
- Get a specific activity's details
- Get the segments of an activity
- Get the leaderboard of a segment

## Installation

### Prerequisites

- Python 3.13 or later
- Strava API credentials (client ID, client secret, and refresh token)

### Setup

1. Clone the repository:

```bash
git clone <repository-url>
cd strava
```

2. Install dependencies with [uv](https://docs.astral.sh/uv/):

```bash
uv install
```

3. Set up environment variables with your Strava API credentials:


Follow the instructions in https://developers.strava.com/docs/getting-started/#curl to get your credentials.

Create a Strava app:

```bash
export STRAVA_CLIENT_ID=your_client_id  ## from https://www.strava.com/settings/api
export STRAVA_CLIENT_SECRET=your_client_secret  ## from https://www.strava.com/settings/api
```

Then, to get the refresh token with appropriate scopes, follow the instructions in https://developers.strava.com/docs/getting-started/#curl.
Please not scope is set to `read_all` so we can get all the data, not only basic profile information.

Access the following URL with your browser to authorize the app:

```bash
echo "http://www.strava.com/oauth/authorize?client_id=$STRAVA_CLIENT_ID&response_type=code&redirect_uri=http://localhost/exchange_token&approval_prompt=force&scope=activity:read_all,read_all" | pbcopy
```


Then, you will be redirected to a URL with a code. Copy the code and paste it into the following command:

```bash
CODE=your_code  ## from the URL
http POST https://www.strava.com/oauth/token \
  client_id=$STRAVA_CLIENT_ID \
  client_secret=$STRAVA_CLIENT_SECRET \
  code=$CODE \
  grant_type=authorization_code
```

This will return a JSON response with the refresh token. Copy the refresh token and paste it into the following command:

```bash
export STRAVA_REFRESH_TOKEN=your_refresh_token
```

Alternatively, you can create a `.env` file in the root directory with these variables.

## Usage

### Running the Server

To run the server:

```bash
python main.py
```

### Development Mode

You can run the server in development mode with the MCP CLI:

```bash
mcp dev main.py
```

### Installing in Claude Desktop

To install the server in Claude Desktop:

```bash
mcp install main.py
```

## Tools

### Get User Activities

Retrieves a list of activities for the authenticated user.

**Parameters:**
- `before` (optional): Epoch timestamp to filter activities before a certain time
- `after` (optional): Epoch timestamp to filter activities after a certain time
- `page` (optional): Page number (default: 1)
- `per_page` (optional): Number of items per page (default: 30)

### Get Activity

Gets detailed information about a specific activity.

**Parameters:**
- `activity_id`: The ID of the activity
- `include_all_efforts` (optional): Whether to include all segment efforts (default: false)

### Get Activity Segments

Retrieves the segments from a specific activity.

**Parameters:**
- `activity_id`: The ID of the activity

### Get Segment Leaderboard

Gets the leaderboard for a specific segment.

**Parameters:**
- `segment_id`: The ID of the segment
- `gender` (optional): Filter by gender ('M' or 'F')
- `age_group` (optional): Filter by age group
- `weight_class` (optional): Filter by weight class
- `following` (optional): Filter by friends of the authenticated athlete
- `club_id` (optional): Filter by club
- `date_range` (optional): Filter by date range
- `context_entries` (optional): Number of context entries
- `page` (optional): Page number (default: 1)
- `per_page` (optional): Number of items per page (default: 30)

## Development

### Project Structure

- `strava_mcp/`: Main package directory
  - `__init__.py`: Package initialization
  - `config.py`: Configuration settings using pydantic-settings
  - `models.py`: Pydantic models for Strava API entities
  - `api.py`: Low-level API client for Strava
  - `service.py`: Service layer for business logic
  - `server.py`: MCP server implementation
- `tests/`: Unit tests
- `main.py`: Entry point to run the server

### Running Tests

To run tests:

```bash
pytest
```

## License

[MIT License](LICENSE)

## Acknowledgements

- [Strava API](https://developers.strava.com/)
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)
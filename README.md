# Toggl MCP Server

Forked from: https://github.com/abhinav24jha/toggl-mcp-server

Allows MCP clients to interact with Toggl Track, enabling time tracking, project management, and workspace operations through natural language.

## Features

### Tools

#### Project Management
- **create_project**
  - **Description**: Creates a new project in a specified Toggl workspace.
  - **Input**:
    - `name` (str): The name of the project to be created. This is a required field.
    - `workspace_name` (str, optional): The name of the workspace where the project will be created. If not provided, it defaults to the user's default workspace.
    - `active` (bool, optional): Specifies whether the project is active. Defaults to `True`.
    - `billable` (bool, optional): Specifies whether the project is billable. Defaults to `False`.
    - `client_id` (int, optional): The ID of the client associated with the project.
    - `color` (str, optional): The hex color code assigned to the project (e.g., "#FF0000").
    - `is_private` (bool, optional): Specifies whether the project is private. Defaults to `True`.
    - `start_date` (str, optional): The start date of the project in ISO 8601 format (e.g., "YYYY-MM-DD").
    - `end_date` (str, optional): The end date of the project in ISO 8601 format (e.g., "YYYY-MM-DD").
    - `estimated_hours` (int, optional): The estimated number of hours for the project.
    - `template` (bool, optional): Specifies whether the project is a template. Defaults to `False`.
    - `template_id` (int, optional): The ID of the template to use for creating the project.
  - **Output**: JSON response containing the data of the newly created project.

- **delete_project**
  - **Description**: Deletes a project identified by its name within a specified workspace.
  - **Input**:
    - `project_name` (str): The exact name of the project to be deleted. This is a required field.
    - `workspace_name` (str, optional): The name of the workspace containing the project. If not provided, it defaults to the user's default workspace.
  - **Output**: A confirmation message (str) indicating the successful deletion of the project.

- **update_projects**
  - **Description**: Performs bulk updates on multiple projects within a specified workspace using patch operations.
  - **Input**:
    - `project_names` (List[str]): A list containing the names of the projects to be updated. This is a required field.
    - `operations` (List[dict]): A list of patch operations to apply to the selected projects. Each operation is a dictionary specifying the operation type (`op`), the field path (`path`), and the new value (`value`) (e.g., `{"op": "replace", "path": "/active", "value": false}`). This is a required field.
    - `workspace_name` (str, optional): The name of the workspace containing the projects. If not provided, it defaults to the user's default workspace.
  - **Output**: JSON response containing the data of the updated projects.

- **get_all_projects**
  - **Description**: Retrieves a list of all projects within a specified workspace.
  - **Input**:
    - `workspace_name` (str, optional): The name of the workspace from which to retrieve projects. If not provided, it defaults to the user's default workspace.
  - **Output**: JSON response containing a list of project data objects found in the specified workspace.

#### Time Entry Management
- **new_time_entry**
  - **Description**: Creates a new time entry. Can be used to start a timer (if only `start` is provided or neither `start` nor `duration` are provided) or log a completed time entry (if `start` and `stop`, or `start` and `duration` are provided).
  - **Input**:
    - `description` (str): The description for the time entry.
    - `workspace_name` (str, optional): Name of the workspace. Defaults to the user's default workspace.
    - `project_name` (str, optional): Name of the project to associate the time entry with.
    - `tags` (List[str], optional): A list of tag names to apply to the time entry.
    - `start` (str, optional): The start time of the entry in ISO 8601 format (e.g., "2023-10-26T10:00:00Z"). Defaults to the current time if creating a running entry.
    - `stop` (str, optional): The stop time of the entry in ISO 8601 format. If provided, creates a completed entry.
    - `duration` (int, optional): The duration of the entry in seconds. If `start` is provided but `stop` is not, `duration` determines the stop time. If `start` is not provided, a negative duration starts a running timer.
    - `billable` (bool, optional): Whether the time entry should be marked as billable. Defaults to False.
    - `created_with` (str, optional): The name of the application creating the entry. Defaults to "MCP".
  - **Output**: JSON response containing the data of the created time entry.

- **stop_time_entry**
  - **Description**: Stops the currently running time entry.
  - **Input**:
    - `workspace_name` (str, optional): Name of the workspace where the entry is running. Defaults to the user's default workspace.
  - **Output**: JSON response containing the data of the stopped time entry.

- **delete_time_entry**
  - **Description**: Deletes a specific time entry by its description and start time.
  - **Input**:
    - `time_entry_description` (str): The exact description of the time entry to delete.
    - `start_time` (str): The exact start time of the entry in ISO 8601 format used for identification.
    - `workspace_name` (str, optional): Name of the workspace containing the entry. Defaults to the user's default workspace.
  - **Output**: Success confirmation message (str) upon successful deletion.

- **get_current_time_entry**
  - **Description**: Fetches the details of the currently running time entry.
  - **Input**: None (implicitly uses the user's context).
  - **Output**: JSON response containing the data of the currently running time entry, or None if no time entry is currently running.

- **update_time_entry**
  - **Description**: Updates attributes of an existing time entry identified by its description and start time.
  - **Input**:
    - `time_entry_description` (str): The current description of the time entry to update.
    - `start_time` (str): The exact start time of the entry in ISO 8601 format used for identification.
    - `workspace_name` (str, optional): Name of the workspace containing the entry. Defaults to the user's default workspace.
    - `new_description` (str, optional): New description for the time entry.
    - `project_name` (str, optional): New project name to associate with the entry. Set to empty string "" to remove project association.
    - `tags` (List[str], optional): A new list of tag names. This will replace all existing tags. Provide an empty list `[]` to remove all tags.
    - `new_start` (str, optional): New start time in ISO 8601 format.
    - `new_stop` (str, optional): New stop time in ISO 8601 format.
    - `billable` (bool, optional): New billable status.
  - **Output**: JSON response containing the data of the updated time entry.

- **get_time_entries_for_range**
  - **Description**: Retrieves time entries within a specified date range, defined by offsets from the current day.
  - **Input**:
    - `from_day_offset` (int): The start day offset from today (e.g., 0 for today, -1 for yesterday, -7 for a week ago).
    - `to_day_offset` (int): The end day offset from today (e.g., 0 for today, 1 for tomorrow). The range includes both the start and end dates.
    - `workspace_name` (str, optional): Name of the workspace to fetch entries from. Defaults to the user's default workspace.
  - **Output**: JSON response containing a list of time entries found within the specified date range.

#### Additional Tools

- **get_organization_users**
  - **Description**: Retrieves an optionally filtered list of users in an organization.
  - **Input**:
    - `workspaces` (List, str, optional): List of workspaces to be searched
    - `name_or_email` (str, optional): The name or email of a user being searched for
    - `active_status` (str, optional): Filter by user status ('active', 'inactive', 'invited')
    - `only_admins` (bool, optional): If true returns admins only
    - `groups` (List, str, optional): List of group IDs. Returns users belonging to these groups
  - **Output**: JSON response containing list of users with their details.

- **search_time_entries_summary_report**
  - **Description**: Creates a summary report of time entries with various filters.
  - **Input**:
    - `workspace_name` (str, optional): Name of the workspace. Defaults to user's default workspace.
    - `start_date` (str, optional): Start date in YYYY-MM-DD format. Should be less than end_date.
    - `end_date` (str, optional): End date in YYYY-MM-DD format. Should be greater than start_date.
    - `start_time` (str, optional): Start time filter.
    - `end_time` (str, optional): End time filter.
    - `description` (str, optional): Description filter for time entries.
    - `project_ids` (List[int], optional): Project IDs to filter by. Use [null] for entries with no projects.
    - `client_ids` (List[int], optional): Client IDs to filter by. Use [null] for entries with no clients.
    - `user_ids` (List[int], optional): User IDs to filter by.
    - `tag_ids` (List[int], optional): Tag IDs to filter by. Use [null] for entries with no tags.
    - `task_ids` (List[int], optional): Task IDs to filter by. Use [null] for entries with no tasks.
    - `time_entry_ids` (List[int], optional): Specific time entry IDs to filter by.
    - `group_ids` (List[int], optional): Group IDs to filter by.
    - `billable` (bool, optional): Filter by billable status (premium feature).
    - `min_duration_seconds` (int, optional): Minimum duration in seconds.
    - `max_duration_seconds` (int, optional): Maximum duration in seconds.
    - `grouping` (str, optional): Grouping option for results.
    - `sub_grouping` (str, optional): Sub-grouping option for results.
    - `distinguish_rates` (bool, optional): Create new subgroups for each rate. Default false.
    - `include_time_entry_ids` (bool, optional): Include time entry IDs in results. Default false.
    - `rounding` (int, optional): Whether time should be rounded.
    - `rounding_minutes` (int, optional): Rounding minutes value. Must be 0, 1, 5, 6, 10, 12, 15, 30, 60 or 240.
  - **Output**: JSON response containing time entry data matching the search criteria in a summary format.

- **search_time_entries_detailed_report**
  - **Description**: Creates a detailed report of time entries with various filters.
  - **Input**:
    - `workspace_name` (str, optional): Name of the workspace. Defaults to user's default workspace.
    - `start_date` (str, optional): Start date in YYYY-MM-DD format. Should be less than end_date.
    - `end_date` (str, optional): End date in YYYY-MM-DD format. Should be greater than start_date.
    - `start_time` (str, optional): Start time filter.
    - `end_time` (str, optional): End time filter.
    - `description` (str, optional): Description filter for time entries.
    - `project_ids` (List[int], optional): Project IDs to filter by. Use [null] for entries with no projects.
    - `client_ids` (List[int], optional): Client IDs to filter by. Use [null] for entries with no clients.
    - `user_ids` (List[int], optional): User IDs to filter by.
    - `tag_ids` (List[int], optional): Tag IDs to filter by. Use [null] for entries with no tags.
    - `task_ids` (List[int], optional): Task IDs to filter by. Use [null] for entries with no tasks.
    - `time_entry_ids` (List[int], optional): Specific time entry IDs to filter by.
    - `group_ids` (List[int], optional): Group IDs to filter by.
    - `billable` (bool, optional): Filter by billable status (premium feature).
    - `min_duration_seconds` (int, optional): Minimum duration in seconds.
    - `max_duration_seconds` (int, optional): Maximum duration in seconds.
    - `grouped` (bool, optional): Whether to group results.
  - **Output**: JSON response containing time entry data matching the search criteria.

- **search_time_entries_weekly_report**
  - **Description**: Creates a weekly report of time entries with various filters.
  - **Input**:
    - `workspace_name` (str, optional): Name of the workspace. Defaults to user's default workspace.
    - `start_date` (str, optional): Start date in YYYY-MM-DD format. Should be less than end_date.
    - `end_date` (str, optional): End date in YYYY-MM-DD format. Should be greater than start_date.
    - `start_time` (str, optional): Start time filter.
    - `end_time` (str, optional): End time filter.
    - `description` (str, optional): Description filter for time entries.
    - `project_ids` (List[int], optional): Project IDs to filter by. Use [null] for entries with no projects.
    - `client_ids` (List[int], optional): Client IDs to filter by. Use [null] for entries with no clients.
    - `user_ids` (List[int], optional): User IDs to filter by.
    - `tag_ids` (List[int], optional): Tag IDs to filter by. Use [null] for entries with no tags.
    - `task_ids` (List[int], optional): Task IDs to filter by. Use [null] for entries with no tasks.
    - `time_entry_ids` (List[int], optional): Specific time entry IDs to filter by.
    - `group_ids` (List[int], optional): Group IDs to filter by.
    - `billable` (bool, optional): Filter by billable status (premium feature).
    - `min_duration_seconds` (int, optional): Minimum duration in seconds.
    - `max_duration_seconds` (int, optional): Maximum duration in seconds.
    - `rounding` (int, optional): Whether time should be rounded.
    - `rounding_minutes` (int, optional): Rounding minutes value. Must be 0, 1, 5, 6, 10, 12, 15, 30, 60 or 240.
  - **Output**: JSON response containing time entry data matching the search criteria.

- **get_project_users**
  - **Description**: Retrieves a list of users in a Toggl project.
  - **Input**:
    - `workspace_name` (str, optional): Workspace to be searched
    - `client_ids` (list, int, optional): List of ID numbers of clients.
    - `project_ids` (list, int, optional): List of project ID numbers.
  - **Output**: JSON response containing list of users with associated projects.

- **get_project_groups**
  - **Description**: Retrieves a list of groups in a Toggl project.
  - **Input**:
    - `workspace_name` (str, optional): Workspace to be searched
    - `client_ids` (list, int, optional): List of ID numbers of clients.
    - `project_ids` (list, int, optional): List of project ID numbers.
  - **Output**: JSON response containing list of groups with associated projects.

- **get_organization_groups**
  - **Description**: Retrieves a list of groups in a Toggl organization.
  - **Input**:
    - `name` (str, optional): Name of group being searched for.
    - `workspace` (str, optional): Workspace ID, returns groups assigned to this workspace.
  - **Output**: JSON response containing list of groups meeting search criteria.

- **get_workspace_clients**
  - **Description**: Retrieves a list of clients in a Toggl workspace.
  - **Input**:
    - `workspace_name` (str, optional): Workspace name to be searched.
    - `ids` (list, int, optional): List of ID numbers of clients.
    - `name` (str, optional): Name of client.
  - **Output**: JSON response containing list of clients meeting search criteria.

- **compare_entries** Harvest entries can be obtained using list_entries from https://github.com/adrian-dotco/harvest-mcp-server.
  - **Description**: Compares a list of entries to a list of entries from another system.
  - **Input**:
    - `format` (list, int): List pair containing 0 and 1s pertaining to which of the entry lists are from toggl and which are from harvest. Should be of the form [0, 0] for Toggl vs Toggl comparisons, [0, 1] for Toggl vs Harvest Comparisons, [1, 1] for Harvest vs Harvest Comparisons.
    - `entries_1` (list, dict): List of JSON objects containing information about time entries from system 1.
    - `entries_2` (list, dict): List of JSON objects containing information about time entries from system 2.
    - `toggl_projects` (dict): JSON object containing information about all Toggl projects. Should be obtained from get_all_projects.
    - `project_mapping` (list, list): List of Lists of project mappings. See prompts.md for instructions and example.
  - **Output**: List of JSON objects pertaining to entries without a match. 'system' parameter in objects denotes which system the entry belongs to. The first object contains total number of hours in each system.

- **get_total_hours_toggl_harvest**
  - **Description**: Gets total hours by system.
  - **Input**:
    - `toggl_entries` (list, dict): List of JSON objects containing information about Toggl time entries. Should be obtained from search_time_entries_detailed_report.
    - `harvest_entries` (list, dict): List of JSON objects containing information about Harvest time entries. Should be obtained from list_entries from https://github.com/broadwing/harvest-mcp-server.
  - **Output**: JSON object for system totals.

## Getting Started

### Prerequisites
- Python 3.11+
- Toggl Track account
- uv installed for dependency management

### Environment Variables

### Installation

First install uv:
    - For MacOS/Linux:
        ```bash
        curl -LsSf https://astral.sh/uv/install.sh | sh
        ```
    - For Windows:
        ```bash
        powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
        ```

Make sure to restart your terminal afterwards to ensure that the uv command gets picked up.

Now let's clone the repository and set up the project:

```bash
git clone [repository-url]
cd mcp_toggl_server
uv venv
uv pip install --all
```

### Integration with Development Tools

#### Claude Setup

1. Configure the MCP Server in `%appdata%/Claude/claude_desktop_config.json`:
```json
{
  "mcpServers": {
    "toggl_mcp_server": {
      "command": "C:\\Users\\USERNAME\\.local\\bin\\uv",
      "args": [
        "--directory",
        "Path to repo\\toggl-mcp-server\\toggl-mcp-server",
        "run",
        "toggl_mcp_server.py",
	"--verbose"
      ],
      "env": {
        "TOGGL_EMAIL": "YOUR TOGGL EMAIL",
	"TOGGL_PASSWORD": "YOUR TOGGL PASSWORD",
	"ORGANIZATION_ID": "YOUR TOGGL ORGANIZATION'S ID"
      }
    }
  }
}
```

2. Install https://github.com/broadwing/harvest-mcp-server.

3. Your claude_desktop_config.json should now look like:
```json
{
  "mcpServers": {
    "toggl_mcp_server": {
      "command": "C:\\Users\\USERNAME\\.local\\bin\\uv",
      "args": [
        "--directory",
        "Path to repo\\toggl-mcp-server\\toggl-mcp-server",
        "run",
        "toggl_mcp_server.py",
	"--verbose"
      ],
      "env": {
        "TOGGL_EMAIL": "YOUR TOGGL EMAIL",
	"TOGGL_PASSWORD": "YOUR TOGGL PASSWORD",
	"ORGANIZATION_ID": "YOUR TOGGL ORGANIZATION'S ID"
      }
    },
    "harvest_mcp_server": {
      "command": "node",
      "args": [
        "Path to repo\\harvest-mcp-server\\build\\index.js"
      ],
      "env": {
        "HARVEST_ACCESS_TOKEN": "YOUR HARVEST ACCESS TOKEN",
        "HARVEST_ACCOUNT_ID": "YOUR HARVEST ACCOUNT ID",
        "STANDARD_WORK_DAY_HOURS": "7.5",
        "TIMEZONE": "America/New_York"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```
4. Restart Claude desktop.

#### Usage
1. Open Claude desktop.

2. Go to the "Projects" tab.

3. Click "New project."

4. Enter a name and description for the project.

5. Click "Create project."

6. In your new project, click the pencil button in the "Instructions" tab.

7. Copy the contents of the "prompts.md" file into the text box.

8. IMPORTANT: At the top of the instructions you will see instructions for the user, follow them and update the project mappings in step 5 with your organization's projects.

9. Delete the FOR USER instruction at the top of the instructions.

10. Click "Save instructions."

11. Enter a prompt in the text entry box in the project. Something to the effect: of "Generate a daily report for [MONTH] [YEAR]."

## License

This MCP server is licensed under the MIT License.
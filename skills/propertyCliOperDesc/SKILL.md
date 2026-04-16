---
name: "cli-anything-promap-property"
description: "CLI for Promap-Property REST API - manage property/geographic data assets"
---

# cli-anything-promap-property

A command-line interface for the Promap-Property REST API, enabling automated
management of property/geographic data assets.

## Installation

```bash
pip install cli-anything-promap-property
```

## Prerequisites

- Promap-Property API service running and accessible
- Python 3.11+
- Valid tenant code and auth token (if required by your API instance)

## Configuration

Configure connection settings (resolved in order):

1. CLI flags: `--host`, `--port`, `--tenant`, `--token`
2. Environment variables: `PROMAP_HOST`, `PROMAP_PORT`, `PROMAP_TENANT`, `PROMAP_TOKEN`
3. Config file: `~/.config/cli-anything-promap-property/config.json`
4. Defaults: `localhost:8080`

```bash
# Save configuration
cli-anything-promap-property --host 10.1.1.100 --port 8080 --tenant mytenant config save
```

## Command Groups

| Group | Commands | Description |
|-------|----------|-------------|
| `config` | show, save, test | Connection configuration |
| `session` | new, status | Session management |
| `data` | detail, list, count, export, reload, download-url, copy | Property data operations |
| `directory` | tree, create, modify, delete, move, list, virtual-create | Directory management |
| `field` | list | Field configuration |
| `import` | progress | Data import operations |
| `event` | list | Data event handling |
| `flow` | status | Workflow management |

## Usage Examples

### Interactive REPL

```bash
cli-anything-promap-property
# Type 'help' for commands, 'quit' to exit
```

### One-shot Commands

```bash
# Get directory tree
cli-anything-promap-property --json directory tree

# Get data detail
cli-anything-promap-property data detail --directory-code DIR001

# Export data to CSV
cli-anything-promap-property data export --directory-code DIR001 --type csv

# Create a new directory
cli-anything-promap-property directory create --parent-code ROOT --name "New Folder"

# Get download URLs for multiple datasets
cli-anything-promap-property data download-url --directory-codes DIR001,DIR002 --type xlsx
```

## JSON Output for Agents

All commands support `--json` flag for machine-readable output:

```bash
cli-anything-promap-property --json directory tree
# Output: {"code": 200, "msg": "success", "data": [...]}
```

## Export Formats

| Format | Code | CLI Option |
|--------|------|------------|
| CSV | 1 | `--type csv` |
| TXT | 2 | `--type txt` |
| XLSX | 3 | `--type xlsx` |
| GeoJSON | 4 | `--type geojson` |
| Shapefile | 5 | `--type shp` |
| WKT | 6 | `--type wkt` |
| GeoPackage | 7 | `--type geopackage` |
| KML | 8 | `--type kml` |
| KMZ | 9 | `--type kmz` |

## Data Format Types

| Type | Code | Description |
|------|------|-------------|
| Point | 0 | Point coordinates |
| Line | 1 | Line/polyline |
| Fence | 2 | Polygon/area |

## Directory Types

| Type | Code | Description |
|------|------|-------------|
| Personal | 1 | User's personal data |
| Enterprise | 2 | Organization data |
| Shared | 3 | Shared directories |

## Error Handling

API errors are returned in standard format:
```json
{
  "code": 400,
  "msg": "gogeo.directory.not.exists",
  "data": null
}
```

CLI exits with non-zero status on errors. Check stderr for error messages.

## Session State

Session file format (JSON):
```json
{
  "api_base": "http://localhost:8080/api/v1/property/",
  "tenant_code": "tenant001",
  "auth_token": "...",
  "current_directory": "DIR001"
}
```

## Agent Integration Tips

1. Use `--json` for all automated operations
2. Test connection before operations: `config test`
3. Get tree first to understand directory structure
4. Use directory codes for all data operations
5. Export formats are numeric codes internally (use names in CLI)

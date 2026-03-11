# Get-message-records

A Python CLI tool to query `message_records` from Supabase.

## Setup

1. Copy `.env.example` to `.env` and fill in your Supabase credentials:
   ```bash
   cp .env.example .env
   ```

2. Install dependencies with [uv](https://docs.astral.sh/uv/):
   ```bash
   uv sync
   ```

## Usage

```bash
uv run get-message-records [OPTIONS]
```

### Options

| Option | Description |
|--------|-------------|
| `--period {1h,4h,12h,24h,7d}` | Preset time range (default: 24h) |
| `--from DATETIME` | Custom start time (ISO 8601, mutually exclusive with --period) |
| `--to DATETIME` | Custom end time (ISO 8601, default: now) |
| `--group GROUP_NAME` | Filter by group_name |
| `--product-type TYPE` | Filter by product_type |
| `--speaker SPEAKER` | Filter by speaker |
| `--keyword TEXT` | Search in message_content (case-insensitive) |
| `--list-groups` | List all distinct group_name values |
| `--list-product-types` | List all distinct product_type values |
| `--format {json,table}` | Output format (default: json) |
| `--limit N` | Max number of records (default: 500) |
| `--supabase-url URL` | Supabase project URL |
| `--supabase-key KEY` | Supabase publishable API key |

### Environment Variables

- `SUPABASE_URL` — Supabase project URL
- `SUPABASE_KEY` — Supabase publishable key

Priority: CLI args > environment variables > `.env` file

## Examples

```bash
# Show help
uv run get-message-records --help

# List all groups
uv run get-message-records --list-groups

# List all product types
uv run get-message-records --list-product-types

# Last 7 days, table format
uv run get-message-records --period 7d --format table

# Filter by group and keyword
uv run get-message-records --group "MyGroup" --keyword "測試" --format table

# Custom time range
uv run get-message-records --from 2026-03-01T00:00:00 --to 2026-03-10T00:00:00 --limit 10

# Filter by speaker
uv run get-message-records --speaker "Alice" --period 24h --format json
```

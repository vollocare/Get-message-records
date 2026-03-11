---
name: get-message-records
description: Query message records from Supabase. Use when the user asks to search, list, or filter message records by time, group, product type, speaker, or keyword.
---

# get-message-records

CLI tool to query `message_records` from Supabase. Outputs JSON (AI-friendly) or table format.

## Prerequisites

The following environment variables must be set:

```bash
export SUPABASE_URL=https://your-project-id.supabase.co
export SUPABASE_KEY=your_publishable_key
```

If not set, provide them via CLI flags: `--supabase-url` and `--supabase-key`.

## Installation

```bash
uv tool install git+https://github.com/vollocare/Get-message-records.git
```

## Command

```bash
get-message-records [OPTIONS]
```

## Parameters

### Time Filters (mutually exclusive)

| Parameter | Description |
|-----------|-------------|
| `--period {1h,4h,12h,24h,7d}` | Preset time range (default: `24h`) |
| `--from DATETIME` | Custom start time (ISO 8601) |
| `--to DATETIME` | Custom end time (default: now) |

If no time parameter is given, defaults to the last 24 hours.

### Content Filters

| Parameter | Description |
|-----------|-------------|
| `--group GROUP_NAME` | Filter by exact `group_name` |
| `--product-type TYPE` | Filter by exact `product_type` |
| `--speaker SPEAKER` | Filter by exact `speaker` |
| `--keyword TEXT` | Search `message_content` (server-side case-insensitive match) |
| `--list-groups` | List all distinct `group_name` values in the database |
| `--list-product-types` | List all distinct `product_type` values in the database |

### Output Control

| Parameter | Description |
|-----------|-------------|
| `--format {json,table}` | Output format (default: `json`) |
| `--limit N` | Max number of records (default: `500`) |

## Examples

```bash
# List all available groups (do this first!)
get-message-records --list-groups

# List all available product types
get-message-records --list-product-types

# Query last 24 hours (default)
get-message-records

# Query last 7 days
get-message-records --period 7d

# Filter by group
get-message-records --period 24h --group "[TWP] IA/SE/MTD/SD/BU群"

# Filter by product type with table output
get-message-records --product-type "波拉西亞戰記" --format table

# Filter by speaker
get-message-records --speaker "Ruby WU(吳琬筑)" --period 7d

# Keyword search
get-message-records --keyword "維護" --format table

# Custom time range
get-message-records --from 2026-03-01T00:00:00 --to 2026-03-10T00:00:00

# Combine filters
get-message-records --group "[TWP] IA/SE/MTD/SD/BU群" --keyword "通行證" --period 7d

# Limit results
get-message-records --period 7d --limit 20
```

## JSON Output Structure

```json
{
  "query": {
    "period": "24h",
    "group_name": null,
    "product_type": null,
    "speaker": null,
    "keyword": null,
    "from": "2026-03-10T08:00:00+00:00",
    "to": "2026-03-11T08:00:00+00:00"
  },
  "count": 5,
  "records": [
    {
      "id": 12,
      "message_time": "2026-03-11T07:57:00+00:00",
      "group_name": "[TWP] IA/SE/MTD/SD/BU群",
      "product_type": "波拉西亞戰記",
      "speaker": "Ruby WU(吳琬筑)",
      "message_content": "...",
      "createtime": "2026-03-11T07:58:47+00:00"
    }
  ]
}
```

## Usage Tips

1. **Start with `--list-groups`** to discover available groups before filtering.
2. **Use `--list-product-types`** to discover available product types.
3. **Use `--format table`** when the user wants a human-readable summary.
4. **Use `--format json`** (default) when you need to process or analyze the data programmatically.
5. **Combine filters**: `--group`, `--product-type`, `--speaker`, and `--keyword` can all be combined.
6. **Time ranges**: Use `--period` for quick queries, `--from`/`--to` for precise ranges.
7. **Parse JSON output** to answer follow-up questions like counts, summaries, or comparisons.

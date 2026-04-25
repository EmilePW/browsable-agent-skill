---
name: browsable
description: "Create and run Browsable scrapers from URLs for agent-based workflows."
---

# Browsable

## Purpose
Use this skill when an agent needs to generate a Browsable scraper from a URL and execute it.

## MCP-first flow

1. Discover existing tasks first with `list_tasks` and `get_task`.
2. Prefer `create_scraper_and_run` when the user wants end-to-end result in one step.
3. For split flow use `create_scraper` then `get_scraper_generation`.
4. Once `get_scraper_generation` returns `completed`, run the returned `generated_task.task_key` with `run_task`.

## Tool behavior

- `create_scraper`
  - Required: `url`
  - Optional: `name`, `description`
  - Starts agent generation and returns `generation_id` + `status_url`.
- `get_scraper_generation`
  - Required: `generation_id`
  - Returns generation lifecycle and `generated_task` when ready.
- `create_scraper_and_run`
  - Required: `url`
  - Optional: `name`, `description`, `run_input`, `run_async`, `wait_for_completion`, `max_wait_ms`, `poll_interval_ms`
  - Returns creation result, generation status, and run result.

## Error handling

- `402`: show remaining credits and ask user to top up.
- `428`: return hosted auth URL and wait for auth completion.
- `404`: invalid generation or task key; ask for corrected input.
- Terminal non-success statuses (`failed`, `errored`, `cancelled`) should stop the flow and return the generation payload.


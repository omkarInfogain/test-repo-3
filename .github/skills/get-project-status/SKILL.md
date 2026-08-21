---
name: get-project-status
description: 'Scan workspace files and return SDLC lifecycle status as structured JSON. Use when the application requests project status or lifecycle dashboard data. This is used in headless mode only.'
allowed-tools: listDirectory readFile fileSearch textSearch
max-steps: 12
metadata:
  risk-level: low
  headless-only: true
---

# Get Project Status

## Overview

Scans a workspace's actual files via MCP filesystem tools (listDirectory, readFile) to determine the current SDLC lifecycle phase status. Returns a structured JSON object conforming to the LifecycleDashboard schema. This is a headless-only skill — no interactive dialogue, no markdown, no follow-up questions.

## Why This Exists

The application's lifecycle dashboard needs accurate, real-time project status. Database artifact metadata is only updated when users explicitly register artifacts through the UI, making it a lagging and incomplete indicator. Scanning the actual workspace files on storage is the source of truth.

## Output Contract

This skill is consumed programmatically by the application. The ENTIRE response must be a single raw JSON object. No prose, no markdown, no code fences, no narration of steps. Silence all intermediate commentary.

## Execution

Perform all steps in ./workflow.md SILENTLY. Only output the final JSON object as specified in step 11.
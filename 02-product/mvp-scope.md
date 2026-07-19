# MVP Scope

## MVP Goal

Create a local-first planning and control loop that supports The Discrepancy Desk without autonomous posting.

## MVP Includes

- local SQLite database
- FastAPI backend
- Jinja/HTMX dashboard
- capture inbox
- sources table
- drafts table
- human review table
- published posts table
- raw capture table
- manual copy/paste publishing workflow
- basic status dashboard
- claim labels
- exact-text approval binding

## MVP Excludes

- X write API
- autonomous posting
- autonomous replies
- auto-like/follow/repost
- Qdrant
- Obsidian sync
- Chrome extension unless M1/M2 expands
- complex analytics
- multi-account UI
- React SPA
- Postgres
- Redis/Celery

## MVP Principle

Small complete loop beats giant unfinished machine.

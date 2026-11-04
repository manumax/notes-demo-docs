# Notes API Product Knowledge Base

This repository is the accepted Product Knowledge Base for the Notes API. It describes the product behaviour that the implementation in `notes-demo-code` must provide.

## Product

The Notes API is a small HTTP API for managing notes in memory. A note has three fields:

| Field | Type | Description |
|---|---|---|
| `id` | integer | Server-assigned identifier |
| `title` | string | Short title for the note |
| `content` | string | Body of the note |

New notes receive a unique integer `id`. The baseline implementation does not persist data across process restarts.

## Operations

### List notes

`GET /notes`

Returns every note as a JSON array. Notes are returned in ascending `id` order.

### Create a note

`POST /notes`

Request body:

```json
{
  "title": "Shopping list",
  "content": "Milk and coffee"
}
```

The `title` and `content` fields are required non-empty strings. The response is the created note, including its assigned `id`, and uses HTTP `201 Created`.

### Retrieve a note

`GET /notes/{id}`

Returns the note with the requested identifier. If no such note exists, the API returns HTTP `404 Not Found`.

### Update a note

`PUT /notes/{id}`

Request body:

```json
{
  "title": "Updated shopping list",
  "content": "Milk, coffee, and tea"
}
```

The request replaces the note's `title` and `content`. Both fields are required non-empty strings. The response is the updated note. If no such note exists, the API returns HTTP `404 Not Found`.

### Delete a note

`DELETE /notes/{id}`

Deletes the note and returns HTTP `204 No Content`. If no such note exists, the API returns HTTP `404 Not Found`.

## Product boundaries

- Storage is in memory for the baseline product.
- The API is intentionally small and has no authentication, user accounts, search, pagination, or persistence requirements yet.
- The implementation should expose FastAPI's generated OpenAPI documentation.

This document is the baseline specification. Product changes should be proposed and reviewed here before the implementation is changed.

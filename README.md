# Notes API Product Knowledge Base

This repository is the accepted Product Knowledge Base for the Notes API. It describes the product behaviour that the implementation in `notes-demo-code` must provide.

## Product

The Notes API is a small HTTP API for managing notes in memory. A note has four fields:

| Field | Type | Description |
|---|---|---|
| `id` | integer | Server-assigned identifier |
| `title` | string | Short title for the note |
| `content` | string | Body of the note |
| `tags` | array of strings | Optional labels used to group notes |

New notes receive a unique integer `id`. The baseline implementation does not persist data across process restarts.

Each tag is a non-empty string, and tags are matched exactly, including case and whitespace. A note's `tags` field is always present in responses as an array; it is empty when the note has no tags. Duplicate tag values on a note are stored once.

## Operations

### List notes

`GET /notes`

Returns every note as a JSON array. Notes are returned in ascending `id` order.

`GET /notes?tag=<value>` returns only notes whose `tags` contain exactly `<value>`, still in ascending `id` order. A `tag` value that matches no note, including an empty value, returns HTTP `200 OK` with an empty array. When `tag` is omitted, all notes are returned.

### Create a note

`POST /notes`

Request body:

```json
{
  "title": "Shopping list",
  "content": "Milk and coffee",
  "tags": ["personal", "shopping"]
}
```

The `title` and `content` fields are required non-empty strings. The `tags` field is optional; if it is omitted, the note is created with no tags. The response is the created note, including its assigned `id`, and uses HTTP `201 Created`.

### Retrieve a note

`GET /notes/{id}`

Returns the note with the requested identifier. If no such note exists, the API returns HTTP `404 Not Found`.

### Update a note

`PUT /notes/{id}`

Request body:

```json
{
  "title": "Updated shopping list",
  "content": "Milk, coffee, and tea",
  "tags": ["personal"]
}
```

The request replaces the note's `title`, `content`, and `tags`. The `title` and `content` fields are required non-empty strings. The `tags` field is optional; if it is omitted or empty, all tags are cleared. The response is the updated note. If no such note exists, the API returns HTTP `404 Not Found`.

### Delete a note

`DELETE /notes/{id}`

Deletes the note and returns HTTP `204 No Content`. If no such note exists, the API returns HTTP `404 Not Found`.

## Product boundaries

- Storage is in memory for the baseline product.
- The API is intentionally small and has no authentication, user accounts, pagination, or persistence requirements yet. It supports filtering notes by one exact tag on `GET /notes`; broader search and multi-tag filtering are not supported.
- The implementation should expose FastAPI's generated OpenAPI documentation.

This document is the baseline specification. Product changes should be proposed and reviewed here before the implementation is changed.
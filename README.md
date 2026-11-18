# Notes API Product Knowledge Base

This repository is the accepted Product Knowledge Base for the Notes API. It describes the product behaviour that the implementation in `notes-demo-code` must provide.

## Product

The Notes API is a small HTTP API for managing notes in memory. A note has five fields:

| Field | Type | Description |
|---|---|---|
| `id` | integer | Server-assigned identifier |
| `title` | string | Short title for the note |
| `content` | string | Body of the note |
| `tags` | array of strings | Optional labels used to group notes |
| `pinned` | boolean | Whether the note is pinned; defaults to `false` |

New notes receive a unique integer `id`. The baseline implementation does not persist data across process restarts.

Each tag is a non-empty string, and tags are matched exactly, including case and whitespace. A note's `tags` field is always present in responses as an array; it is empty when the note has no tags. Duplicate tag values on a note are stored once.

A note's `pinned` field is always present in responses. It is `false` unless the note has been pinned.

## Operations

### List notes

`GET /notes`

Returns every note as a JSON array. Pinned notes are returned first, followed by the remaining notes. Within each group, notes are returned in ascending `id` order.

`GET /notes?tag=<value>` returns only notes whose `tags` contain exactly `<value>`, in the same order: pinned notes first, followed by the remaining notes, with ascending `id` order within each group. A `tag` value that matches no note, including an empty value, returns HTTP `200 OK` with an empty array. When `tag` is omitted, all notes are returned.

`GET /notes?q=<text>` returns only notes whose `title` contains `<text>` as a substring, in the same order: pinned notes first, followed by the remaining notes, with ascending `id` order within each group. Matching ignores case for the ASCII letters `A`–`Z` against `a`–`z`; all other characters are compared as-is, with no Unicode case folding and no diacritic folding, so `q=cafe` does not match a title containing `Café`. The match is a plain literal substring anywhere in the `title`: a title that equals `<text>` matches, and there are no word-boundary, prefix-only, ranking, or pattern-matching rules. Only `title` is searched; `content` and `tags` are never matched by `q`.

The `q` value is trimmed of surrounding whitespace before matching. When `q` is omitted, empty, or whitespace-only (for example `GET /notes?q=%20%20`), no title filter is applied and the endpoint behaves as if `q` were absent. The same applies to `q` supplied without a value. A `q` that matches no note returns HTTP `200 OK` with an empty array. If `q` is supplied more than once, the last value is used.

When `q` and `tag` are both supplied, the two filters combine: only notes that satisfy both are returned, in the same order. Trimming applies to `q` only; `tag` remains an exact match, so an empty `tag` (for example `GET /notes?q=foo&tag=`) matches no note and returns HTTP `200 OK` with an empty array.

For example, `GET /notes?q=MILK` returns the notes whose titles contain `milk`, `Milk`, or `MILK`, and `GET /notes?q=milk&tag=shopping` returns only the notes whose titles contain `milk` and whose `tags` contain exactly `shopping`, still pinned notes first and then in ascending `id` order.

### Create a note

`POST /notes`

Request body:

```json
{
  "title": "Shopping list",
  "content": "Milk and coffee",
  "tags": ["personal", "shopping"],
  "pinned": true
}
```

The `title` and `content` fields are required non-empty strings. The `tags` field is optional; if it is omitted, the note is created with no tags. The `pinned` field is optional; if it is omitted, the note is created with `pinned` set to `false`. The response is the created note, including its assigned `id`, and uses HTTP `201 Created`.

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
  "tags": ["personal"],
  "pinned": false
}
```

The request replaces the note's `title`, `content`, `tags`, and `pinned`. The `title` and `content` fields are required non-empty strings. The `tags` field is optional; if it is omitted or empty, all tags are cleared. The `pinned` field is optional; if it is omitted, `pinned` is reset to `false`. The response is the updated note. If no such note exists, the API returns HTTP `404 Not Found`.

### Delete a note

`DELETE /notes/{id}`

Deletes the note and returns HTTP `204 No Content`. If no such note exists, the API returns HTTP `404 Not Found`.

## Product boundaries

- Storage is in memory for the baseline product.
- The API is intentionally small and has no authentication, user accounts, pagination, or persistence requirements yet. It supports filtering notes by one exact tag and searching note titles by case-insensitive substring on `GET /notes`; search over note `content`, search over `tags`, multi-tag filtering, and pagination are not supported.
- The implementation should expose FastAPI's generated OpenAPI documentation.

This document is the baseline specification. Product changes should be proposed and reviewed here before the implementation is changed.

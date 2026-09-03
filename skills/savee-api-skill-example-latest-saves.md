# Read the user's latest 10 saves

Returns the 10 most recent saves the authenticated user has made, newest first. Includes their private saves. Use this when the user wants to display, export, or process their own Savee content.

## Request

```bash
curl 'https://api.savee.com/v1/saves?limit=10' \
  -H "Authorization: Bearer $SAVEE_TOKEN"
```

## Response shape (truncated)

```json
{
  "data": [
    {
      "id": "67aada20d242ec0009400825",
      "url": "https://savee.com/i/aB3cD/",
      "name": "Brutalist poster",
      "source_url": "https://example.com/poster",
      "created_at": "2026-05-08T12:00:00.000Z",
      "is_private": false,
      "total_saves": 42,
      "media": {
        "type": "image",
        "width": 1600,
        "height": 2400,
        "thumbnail": "https://dm.savee.com/.../poster-thumb.avif",
        "original": "https://dm.savee.com/.../poster.avif"
      },
      "colors": [
        { "color": "#1A1A1A", "amount": 0.62 },
        { "color": "#E33124", "amount": 0.21 }
      ]
    }
  ],
  "next_cursor": "eyJjIjoiMjAyNi0wNS0wOFQxMjowMDowMFoiLCJpIjoiNjdhYWRhMjAifQ",
  "has_more": true
}
```

## Notes

- `total_saves` is how many users across Savee have saved this same asset (a popularity signal, not the user's count).
- `is_private` marks the save itself as private. It's independent of board privacy: a save can be public but sit in a private board.
- If the user has enabled *Hide private board saves from profile feed* in their Savee settings, saves that belong to a private board are left out of this endpoint too. That's the one case where `/v1/saves` doesn't return everything.
- `media.thumbnail` and `media.original` are AVIF by default. Send `Avif-Fallback: 1` as a request header to receive JPG URLs instead. Video originals are always MP4.
- `colors` lists the dominant colors of the media as hex + coverage fraction (0–1), most prominent first. It is `[]` when colors haven't been extracted for the asset yet.
- The `user` field is **omitted** on `/v1/saves` because the caller is implicitly the author. It is present on `/v1/feed` and `/v1/boards/{id}/saves`.
- For more than 10, paginate by passing `next_cursor` back as `?cursor=…` until `has_more` is `false`. Cursors are opaque — never parse them.

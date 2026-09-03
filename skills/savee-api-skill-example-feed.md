# Read saves from the user's /feed

Returns the saves on the authenticated user's home feed — content from the people they follow, newest first. Each save includes a `user` field describing who saved it (since the author isn't the caller).

## Request

```bash
curl 'https://api.savee.com/v1/feed?limit=20' \
  -H "Authorization: Bearer $SAVEE_TOKEN"
```

## Response shape (truncated)

```json
{
  "data": [
    {
      "id": "67b0e4f1d242ec0009412c73",
      "url": "https://savee.com/i/xY7zQ/",
      "name": "Acid-green typography",
      "source_url": null,
      "created_at": "2026-05-11T18:42:00.000Z",
      "is_private": false,
      "total_saves": 8,
      "media": {
        "type": "image",
        "width": 2000,
        "height": 1333,
        "thumbnail": "https://dm.savee.com/.../typo-thumb.avif",
        "original": "https://dm.savee.com/.../typo.avif"
      },
      "colors": [
        { "color": "#B4F22E", "amount": 0.54 },
        { "color": "#101010", "amount": 0.38 }
      ],
      "user": {
        "id": "5d3f9a17d242ec000931b8ca",
        "username": "saver42",
        "name": "Saver Forty-Two",
        "url": "https://savee.com/saver42/",
        "avatar_url": "https://dm.savee.com/user-avatar/original/8kQ2mZp.jpg"
      }
    }
  ],
  "next_cursor": "eyJjIjoiMjAyNi0wNS0xMVQxODo0MjowMFoiLCJpIjoiNjdiMGU0ZjEifQ",
  "has_more": true
}
```

## Notes

- `user` is always present here — branch on `user.username` or `user.id` if you need to attribute or group by saver.
- The feed reflects the user's "Suggested curation" setting on savee.com. If they have it disabled, the feed only shows people they explicitly follow; if enabled, it also includes platform-curated content.
- Pagination follows the same shape as every other list endpoint: pass `next_cursor` back as `?cursor=…` until `has_more` is `false`.

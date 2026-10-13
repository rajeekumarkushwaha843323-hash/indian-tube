# Admin API quick reference

## Login
`POST /api/admin/login`
```json
{"username":"YOUR_ADMIN_USERNAME","password":"YOUR_ADMIN_PASSWORD"}
```
Response contains a short-lived bearer token.

## Upload movie
`POST /api/admin/movies` (multipart/form-data)
- `title`: required
- `description`: optional
- `posterUrl`: optional HTTPS URL
- `video`: optional file (MP4, WebM or MOV)
- `videoUrl`: optional HTTPS video URL if no file is uploaded
- `rightsConfirmed`: must be the string `true`

Send `Authorization: Bearer <token>`.

## List movies as admin
`GET /api/admin/movies`

## Delete movie
`DELETE /api/admin/movies/{id}` with admin bearer token.

## Report copyright concern
`POST /api/reports/copyright`
```json
{
  "contentId": "movie-id",
  "claimantName": "Name",
  "contactEmail": "rights@example.com",
  "details": "Explain the claimed rights and concern"
}
```
This starter only logs reports to the server console. Production must store reports durably and create a real review/takedown workflow.

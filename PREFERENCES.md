# Preferences

**Client apps** MUST respect user preferences, which can be obtained by `GET /envelopes` request with proper filter. Only a Statement with latest dateCreated property MUST be respected.

```json
{
    "@context": "https://schema.org",
    "@type": "Statement",
    // Optional - if Statement applies only to specific application.
    // "about": {
    //     "@type": "SoftwareApplication",
    //     "@id": "https://example.com/apps/my-app",
    //     "name": "My App"
    // },
    "dateCreated": "2026-02-03T10:15:00Z",
    "text": "I want my settings to be set like this by default.",
    "hasPart": [
        {
            "@type": "PropertyValue",
            "name": "theme",
            "value": "dark"
        },
        {
            "@type": "PropertyValue",
            "name": "language",
            "value": "en"
        },
        {
            "@type": "PropertyValue",
            "name": "notificationsEnabled",
            "value": false
        },
        {
            "@type": "PropertyValue",
            "name": "preferredReadNodes",
            "value": "https://node.example.com"
        },
        {
            "@type": "PropertyValue",
            "name": "preferredWriteNodes",
            "value": "https://node.example.com"
        },
        {
            "@type": "PropertyValue",
            "name": "blockedNodes",
            "value": "https://malicious-node.example.com,https://slow-node.example.com"
        }
    ]
}
```

# Atlas - Preferences

**Client apps** MUST respect user preferences retrieved via `GET /envelopes` with an appropriate filter.  
Only the `Statement` with the latest `dateCreated` value MUST be applied.

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
            "name": "readNodes",
            "value": "https://node.example.com"
        },
        {
            "@type": "PropertyValue",
            "name": "writeNodes",
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

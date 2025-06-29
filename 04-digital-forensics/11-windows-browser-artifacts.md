# 4.11: Windows Browser Artifacts

Browser artifacts represent one of the richest sources of forensic evidence in modern investigations. These artifacts reveal user behavior, visited websites, downloaded files, and cached content that can be crucial for timeline reconstruction and user attribution.

---

## Browser Artifact Categories

Browser forensics focuses on extracting evidence from the three most popular Windows browsers: Microsoft Edge, Google Chrome, and Mozilla Firefox.

### Key Artifact Types

| Artifact Type | Description | Forensic Value |
|---------------|-------------|----------------|
| **Cookies** | Small data files storing website preferences and session info | Track user authentication and site interactions |
| **Favorites/Bookmarks** | User-saved website shortcuts | Reveal interests and frequently accessed sites |
| **Download History** | Record of files downloaded through the browser | Identify potentially malicious files or data exfiltration |
| **URL History** | Complete record of visited websites | Build comprehensive timeline of user activity |
| **Search Queries** | Terms entered into search engines | Understand user intent and information gathering |
| **Cached Webpages** | Locally stored copies of visited sites | Reconstruct what user actually saw on compromised sites |
| **Cached Images** | Downloaded images from websites | Identify accessed content and advertising exposure |

---

## Live Acquisition Methods

### Method 1: KAPE Collection

**KAPE (Kroll Artifact Parser and Extractor)** provides automated collection of browser artifacts from live systems.

#### Collection Workflow
1. **Configure KAPE**:
   - Set target directory to system drive (e.g., `C:\`)
   - Define output destination (e.g., `Desktop/KAPE Browser Forensics`)

2. **Select Browser Targets**:
   - Enable Chrome target
   - Enable Firefox target  
   - Enable Edge target

3. **Execute Collection**:
   - Click "Execute!" to begin automated acquisition
   - Typical collection time: 30-60 seconds

#### Key File Locations Retrieved
- **Chrome**: `C:\Users\[USER]\AppData\Local\Google\Chrome\User Data`
- **Firefox**: `C:\Users\[USER]\AppData\Roaming\Mozilla\Firefox\Profiles`
- **Edge**: `C:\Users\[USER]\AppData\Local\Microsoft\Edge\User Data`

### Method 2: Browser History Capturer (BHC)

**Browser History Capturer** overcomes file locking issues that prevent direct access to browser databases on live systems.

#### Why BHC is Necessary
- Browsers lock database files while running
- Even administrative privileges cannot access locked Edge files
- BHC forces collection of all critical browser forensic files

#### Collection Process
1. **Run BHC with elevation**
2. **Select target user profile**
3. **Verify browser and data options are enabled**
4. **Set capture destination** (default: `Capture` folder)
5. **Execute capture process**

---

## Analysis with Browser History Viewer (BHV)

**Browser History Viewer** provides comprehensive analysis capabilities for collected browser data.

### Interface Layout

| Pane | Function | Contents |
|------|----------|----------|
| **Pane 1** | Primary Data Display | Website History, Cached Images, Cached Web Pages |
| **Pane 2** | Visual Analytics | Website visit count graphs and cached content preview |
| **Pane 3** | Filtering Controls | Keyword search, date range, browser type filters |

### Analysis Capabilities

#### Website History Tab
- **Visit timestamps** with precise date/time
- **Website titles** and full URLs
- **Visit frequency counters** showing user behavior patterns
- **Browser identification** for multi-browser environments

#### Cached Images Tab
- **Image URLs** showing source locations
- **Visual preview** of downloaded images
- **Advertisement tracking** through cached ad images
- **User activity correlation** via image timestamps

#### Cached Web Pages Tab
- **Complete webpage reconstruction** showing user's actual view
- **Search query visualization** displaying exact search terms
- **Temporal browsing context** with precise timing
- **Visual evidence** of user interaction with malicious sites

### Practical Analysis Example

**Scenario**: Investigating potential malware download

1. **Filter by suspicious timeframe** using date controls
2. **Search for download-related keywords** (e.g., "download", "install")
3. **Examine cached pages** around suspected download time
4. **Correlate cached images** with malicious advertisements
5. **Build timeline** of user actions leading to compromise

---

## Forensic Considerations

### File Location Mapping

```
Browser Data Locations:
├── Chrome
│   └── C:\Users\[USER]\AppData\Local\Google\Chrome\User Data\Default\
│       ├── History (SQLite database)
│       ├── Cookies (SQLite database)
│       ├── Cache\ (cached files)
│       └── Bookmarks (JSON file)
├── Firefox
│   └── C:\Users\[USER]\AppData\Roaming\Mozilla\Firefox\Profiles\[PROFILE]\
│       ├── places.sqlite (history/bookmarks)
│       ├── cookies.sqlite
│       └── cache2\ (cached files)
└── Edge
    └── C:\Users\[USER]\AppData\Local\Microsoft\Edge\User Data\Default\
        ├── History (SQLite database)
        ├── Cookies (SQLite database)
        └── Cache\ (cached files)
```

### Collection Best Practices

- **Always use live acquisition tools** (KAPE/BHC) on running systems
- **Verify data integrity** by cross-referencing multiple artifact types
- **Document browser versions** for compatibility with analysis tools
- **Preserve original file timestamps** during collection
- **Consider privacy settings** that may limit artifact availability

### Analysis Limitations

- **Private/Incognito browsing** leaves minimal artifacts
- **Regular cache clearing** reduces available evidence
- **Browser updates** may change database schemas
- **User privacy tools** can sanitize browsing history

[⬆️ Back to Digital Forensics](./README.md)

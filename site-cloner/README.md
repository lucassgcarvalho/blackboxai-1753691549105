# Roadmap.sh Site Cloner

A comprehensive web crawler and site cloning tool specifically designed to clone the entire roadmap.sh website including all sublinks, resources, and dynamically loaded content.

## Features

- **Complete Site Cloning**: Downloads all HTML, CSS, JavaScript, images, fonts, and other resources
- **Dynamic Content Support**: Waits for JavaScript execution and backend responses
- **Recursive Link Discovery**: Follows all internal links and discovers resources dynamically
- **Robust Error Handling**: Retry mechanisms and comprehensive error logging
- **Progress Monitoring**: Real-time progress tracking and detailed reporting
- **Resource Filtering**: Configurable file type and domain filtering
- **Analytics Filtering**: Automatically excludes tracking and analytics requests
- **Lazy Loading Support**: Triggers lazy-loaded content by scrolling
- **Network Interception**: Captures all network requests and responses
- **Metadata Preservation**: Maintains original directory structure and file relationships

## Installation

1. Clone or download this project
2. Navigate to the project directory
3. Install dependencies:

```bash
npm install
```

## Usage

### Basic Usage

```bash
npm start
```

### Clean Previous Output

```bash
npm start -- --clean
```

### Test Mode (Homepage Only)

```bash
npm run test
```

## Configuration

Edit `src/config.js` to customize the crawling behavior:

- **BASE_URL**: Target website URL
- **OUTPUT_DIR**: Directory to save cloned files
- **ALLOWED_EXTENSIONS**: File types to download
- **MAX_CONCURRENCY**: Number of concurrent requests
- **PAGE_WAIT_OPTIONS**: Browser wait conditions
- **RETRY_LIMIT**: Number of retry attempts
- **EXCLUDE_PATTERNS**: URL patterns to skip

## Analytics/Tracking Exclusion

The site cloner automatically filters out analytics and tracking requests to focus on actual website functionality. The following types of resources are excluded:

### Excluded Domains/Services:
- Google Analytics (`analytics.google.com`, `google-analytics.com`)
- Google Tag Manager (`googletagmanager.com`)
- HubSpot tracking (`track.hubspot.com`, `hubspot.com`)
- Google Ads (`adtrafficquality.google`, `pagead2.googlesyndication.com`)
- Facebook tracking (`facebook.com/tr`, `connect.facebook.net`)
- Twitter/X tracking (`twitter.com/i/adsct`, `ads.twitter.com`)
- Amazon advertising (`adsystem.amazon`, `amazon-adsystem.com`)
- Error tracking services (Sentry, Rollbar, Bugsnag, etc.)
- Analytics services (Hotjar, Mixpanel, Segment, Amplitude, etc.)
- Monitoring services (New Relic, Datadog, Pingdom, etc.)

### Excluded URL Patterns:
- Tracking parameters (`utm_*`, `fbclid`, `gclid`, `_ga`, etc.)
- Analytics endpoints (`/collect`, `/analytics/`, `/tracking/`, `/beacon`, etc.)
- Pixel tracking and impression URLs

### Implementation:
- **Request Interception**: Puppeteer aborts requests to excluded domains before they're sent
- **Link Filtering**: Parser excludes analytics URLs when extracting links from content
- **Download Protection**: Downloader has additional safety checks to prevent downloading excluded resources

This filtering significantly reduces unnecessary network traffic and focuses the clone on functional website assets.

## Output Structure

```
output/
├── index.html                 # Main homepage
├── frontend/                  # Frontend roadmap
├── backend/                   # Backend roadmap
├── devops/                    # DevOps roadmap
├── [other-roadmaps]/          # All other roadmaps
├── assets/                    # CSS, JS, images
├── clone-report.json          # Detailed technical report
├── download-report.html       # Visual download report
├── download-metadata.json     # Download metadata
└── README.md                  # Clone information
```

## How It Works

1. **Initialization**: Sets up browser, directories, and logging
2. **Page Processing**: 
   - Loads each page with Puppeteer
   - Waits for all scripts and network requests to complete
   - Extracts all links from HTML and dynamic content
   - Downloads the page and all discovered resources
3. **Resource Discovery**:
   - Parses HTML for static links
   - Intercepts network responses for dynamic resources
   - Triggers lazy loading by scrolling
   - Extracts URLs from CSS, JavaScript, and JSON responses
4. **Download Management**:
   - Downloads files with proper retry logic
   - Maintains original directory structure
   - Handles various file types and content types
5. **Reporting**: Generates comprehensive reports and statistics

## Key Components

- **`crawler.js`**: Main crawling logic with Puppeteer
- **`parser.js`**: Link extraction from various content types
- **`downloader.js`**: File download and saving functionality
- **`utils.js`**: Logging, retry logic, and helper functions
- **`config.js`**: Configuration settings
- **`index.js`**: Main entry point and orchestration

## Advanced Features

### Dynamic Content Handling
- Waits for `networkidle2` condition
- Scrolls to trigger lazy loading
- Intercepts all network responses
- Extracts links from AJAX responses

### Error Recovery
- Configurable retry attempts
- Exponential backoff
- Comprehensive error logging
- Graceful failure handling

### Resource Management
- Memory-efficient processing
- Concurrent download limiting
- File size validation
- Content type detection

## Monitoring and Logging

- Real-time progress bars
- Detailed console logging
- File-based log storage
- Statistics tracking
- Error reporting

## Requirements

- Node.js 16+ (for ES modules support)
- Chrome/Chromium (automatically installed with Puppeteer)
- Sufficient disk space for the cloned site
- Stable internet connection

## Troubleshooting

### Common Issues

1. **Memory Issues**: Reduce `MAX_CONCURRENCY` in config
2. **Timeout Errors**: Increase `PAGE_WAIT_OPTIONS.timeout`
3. **Missing Files**: Check `ALLOWED_EXTENSIONS` configuration
4. **Permission Errors**: Ensure write permissions for output directory

### Debug Mode

Set `LOG_LEVEL: 'debug'` in config for verbose logging.

## Performance Tips

- Use SSD storage for better I/O performance
- Increase `MAX_CONCURRENCY` for faster downloads (but watch memory usage)
- Use `--clean` flag to start fresh
- Monitor system resources during crawling

## Legal and Ethical Considerations

- Respect robots.txt and rate limiting
- Only clone sites you have permission to clone
- Be mindful of server load
- Follow website terms of service

## License

This tool is provided as-is for educational and legitimate use cases only.

---

*Built with Node.js, Puppeteer, and Cheerio*

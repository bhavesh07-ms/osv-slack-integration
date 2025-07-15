OSV to Slack Integration

This standalone Java application integrates the Open Source Vulnerabilities (OSV) Scanner with Slack to monitor a package.json file for npm package vulnerabilities and notify a specified Slack user via direct messages (DMs). It periodically checks for updates to package.json, scans new or updated packages using the OSV API, and sends formatted Slack DMs with vulnerability details or a confirmation of no vulnerabilities.

Setup and Running Instructions

Prerequisites





Java: JDK 11 or higher



Maven: For dependency management



Docker: For containerized deployment



OSV API: No API key required (uses public https://api.osv.dev/v1/query)



Slack: A Slack workspace with a bot token (scopes: chat:write, users:read)



Node.js: A package.json file for testing (e.g., with express or lodash)

Installation





Clone the Repository:

git clone <local-repository>
cd osv-slack-integration



Configure Environment Variables: Create a .env file in the project root with:

SLACK_TOKEN=your-slack-bot-token
SLACK_USER_ID=your-slack-user-id
PACKAGE_JSON_PATH=/path/to/package.json
POLLING_INTERVAL_SECONDS=900





Obtain SLACK_TOKEN from your Slack app (create at api.slack.com/apps).



Find SLACK_USER_ID using Slack’s users.list API or user profile.



Set PACKAGE_JSON_PATH to a local package.json file.



Build the Application:

mvn clean install



Run Locally:

java -jar target/osv-slack-integration-1.0.jar



Run with Docker:

docker build -t osv-slack-integration .
docker run --env-file .env -v /path/to/package.json:/app/package.json osv-slack-integration

Dependencies





Spring Boot: Web, RestTemplate for API calls



Jackson: JSON parsing for package.json and OSV responses



SLF4J/Logback: Logging to file



JUnit/Mockito: Unit testing



Dotenv: Environment variable management

Application Architecture

The application follows a modular, event-driven design for scalability and maintainability:





Config Module (config/): Loads environment variables (e.g., Slack token, polling interval) using dotenv-java.



File Watcher Module (watcher/): Polls package.json for changes using Java’s WatchService or timestamp checks, triggering scans for new/updated packages.



Parser Module (parser/): Parses package.json with Jackson to extract npm package names and versions.



OSV Scanner Module (scanner/): Queries the OSV API (https://api.osv.dev/v1/query) to retrieve vulnerability details for each package.



Slack Notifier Module (notifier/): Sends formatted DMs to a Slack user via chat.postMessage API.



Logging Module (logging/): Uses SLF4J/Logback to log events (info, error) to app.log.



Test Module (test/): JUnit/Mockito tests for parsing, scanning, and notification logic.

The application runs as a Spring Boot service, with a scheduled task for polling and modular components for easy extension (e.g., adding email notifications).

Example Slack Notifications

With Vulnerabilities

Package: express@4.17.1
Vulnerabilities:
- ID: CVE-2023-12345, Summary: Cross-site scripting in express middleware
- ID: CVE-2023-67890, Summary: Denial of service via malformed input
Scan time: 2025-07-15T11:20:00Z

No Vulnerabilities

Package: lodash@4.17.21
The package is free from any vulnerabilities.
Scan time: 2025-07-15T11:20:00Z

AI Tool Usage

This project was developed with assistance from Grok 3, created by xAI, for:





Generating initial code snippets for OSV API queries and Slack DM formatting.



Structuring the modular architecture and suggesting logging best practices.



Providing guidance on Java’s WatchService for file polling. All code was manually reviewed, tested, and adapted to ensure production-ready quality.

Testing

Run unit tests with:

mvn test

Tests cover:





Parsing package.json to extract dependencies.



Formatting Slack messages for vulnerable and clean packages.



Mocking OSV API responses to validate vulnerability parsing.

Notes





The application checks package.json every 15 minutes (configurable via POLLING_INTERVAL_SECONDS).



Logs are written to logs/app.log for troubleshooting.



Do not publish this code publicly, as per Metron Security’s instructions.

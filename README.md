📦 OSV to Slack Integration
A standalone Java application that monitors vulnerabilities in a package.json file using the OSV API and sends alerts via Slack DMs.

🚀 Features
- Periodically monitors changes in package.json
- Scans updated packages for vulnerabilities using the OSV API
- Sends cleanly formatted Slack DM alerts for vulnerable or safe packages
- Modular, event-driven Spring Boot architecture
- 
🛠️ Prerequisites
- Java JDK 11 or higher
- Maven
- Docker (optional for containerization)
- Slack bot token with `chat:write`, `users:read` scopes
- Node.js and a valid package.json file
- 
📦 Installation & Running
1️⃣ Clone the Repository
git clone <repository-url>
cd osv-slack-integration
2️⃣ Set Up Environment Variables (.env)
SLACK_TOKEN=your-slack-bot-token
SLACK_USER_ID=your-slack-user-id
PACKAGE_JSON_PATH=/path/to/package.json
POLLING_INTERVAL_SECONDS=900
3️⃣ Build the Project
mvn clean install
4️⃣ Run Locally
java -jar target/osv-slack-integration-1.0.jar
5️⃣ Run with Docker
docker build -t osv-slack-integration .
docker run --env-file .env -v /path/to/package.json:/app/package.json osv-slack-integration

🧱 Architecture Overview
- **Config Module**: Loads env variables using dotenv-java
- **File Watcher**: Monitors package.json changes
- **Parser Module**: Extracts package/version from JSON
- **OSV Scanner**: Calls OSV API for vulnerability data
- **Slack Notifier**: Sends formatted alerts to user
- **Logger**: Logs to `logs/app.log`
- **Tests**: Unit tests using JUnit & Mockito
- 
📨 Example Slack Alerts
Vulnerable Package:
Package: express@4.17.1
- CVE-2023-12345: Cross-site scripting
- CVE-2023-67890: Denial of service
Scan time: 2025-07-15T11:20:00Z
Clean Package:
Package: lodash@4.17.21
No vulnerabilities found.
Scan time: 2025-07-15T11:20:00Z
🤖 AI Tool Usage
Used Grok 3 (by xAI) for initial code generation and architecture guidance. All code was manually reviewed and tested.

✅ Testing
Run unit tests with:
mvn test
Tests include:
- JSON parsing
- OSV response mocking
- Slack message formatting
⚠️ Notes
- Default polling: 15 minutes (can be configured)
- Logs stored at `logs/app.log`
- Internal use only. Do not publish externally.

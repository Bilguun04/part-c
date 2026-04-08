# SonarQube Static Analysis – Step-by-Step Guide
# ECSE-429 Part C

## Prerequisites
- Java 11 or 17 installed (required by SonarQube 9.9 LTS)
- Maven or the SonarQube scanner CLI
- The runTodoManagerRestAPI source code (from GitHub: https://github.com/eviltester/thingifier)

---

## Step 1 – Download SonarQube 9.9 LTS

Go to:
  https://www.sonarsource.com/products/sonarqube/downloads/historical-downloads/

Download:  SonarQube Community Edition 9.9.x LTS
Extract to a folder, e.g.:  C:\sonarqube-9.9.0  (Windows)  or  ~/sonarqube-9.9.0  (Mac/Linux)

---

## Step 2 – Start SonarQube

### Windows
  cd C:\sonarqube-9.9.0\bin\windows-x86-64
  StartSonar.bat

### Mac / Linux
  cd ~/sonarqube-9.9.0/bin/linux-x86-64
  ./sonar.sh start

Wait ~60 seconds, then open:  http://localhost:9000
Default login:  admin / admin   (you will be prompted to change it on first login)

---

## Step 3 – Get the Thingifier Source Code

  git clone https://github.com/eviltester/thingifier.git
  cd thingifier

The REST API todo manager is in the sub-module:  todoManagerRestAuto/

---

## Step 4 – Create a SonarQube Project

1. Log in at http://localhost:9000
2. Click "Create Project" → "Manually"
3. Project key:    thingifier-todo-api
   Display name:   Thingifier Todo Manager REST API
4. Click "Set Up"
5. Choose "Locally" → generate a token (save it, e.g.  sqp_abc123...)
6. Select "Maven" as the build technology

---

## Step 5 – Run the Scanner

### Using Maven (recommended)
  cd thingifier/todoManagerRestAuto

  mvn sonar:sonar \
    -Dsonar.projectKey=thingifier-todo-api \
    -Dsonar.host.url=http://localhost:9000 \
    -Dsonar.login=<YOUR_TOKEN>

### Using the SonarQube Scanner CLI (alternative)
Download scanner from:  https://docs.sonarqube.org/9.9/analyzing-source-code/scanners/sonarscanner/

Create sonar-project.properties in the project root:
  sonar.projectKey=thingifier-todo-api
  sonar.projectName=Thingifier Todo Manager REST API
  sonar.projectVersion=1.5.5
  sonar.sources=src/main/java
  sonar.java.binaries=target/classes
  sonar.host.url=http://localhost:9000
  sonar.login=<YOUR_TOKEN>

Then run:
  sonar-scanner

---

## Step 6 – View Results

Go to:  http://localhost:9000/dashboard?id=thingifier-todo-api

### Key Metrics to Screenshot and Report:

1. Overview dashboard
   - Bugs, Vulnerabilities, Security Hotspots, Code Smells
   - Technical debt (hours/days estimate)
   - Coverage, Duplications

2. Issues tab  →  filter by:
   - Severity: Major / Critical / Blocker
   - Type: Code Smell
   - Type: Bug

3. Measures tab  →  Complexity section:
   - Cyclomatic Complexity (per file and total)
   - Cognitive Complexity

4. Measures tab  →  Size section:
   - Lines of Code
   - Number of Functions / Classes

5. Duplications tab
   - Duplicated lines (%)
   - Duplicated blocks

### Take screenshots of:
  [ ] Main dashboard overview
  [ ] Top 5 most complex files (sorted by cyclomatic complexity)
  [ ] Top code smells list
  [ ] Bugs/Vulnerabilities panel
  [ ] Technical debt summary

---

## Typical Findings to Expect (Thingifier codebase)

Based on the known structure of the thingifier project:

- HIGH cyclomatic complexity in routing/handler classes  
  (many nested conditionals handling HTTP methods)
- Code smells: long methods in RestMicroServer.java and related handlers
- Some duplicated error handling patterns across todo / project / category endpoints
- Minor: unused imports, magic string literals for HTTP status codes
- Technical debt estimate: typically 1–3 days for a project this size

---

## Reporting Requirements (per project spec)

Your Part C report must include:
  ✓ Description of how SonarQube was set up and run
  ✓ Screenshots of the dashboard (overview + complexity + code smells)
  ✓ List of top issues found with severity and location
  ✓ Recommendations for code improvements (see report template)

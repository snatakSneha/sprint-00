# SOP for JQ – JSON Query Processor

---

### Author

| Created     | Version | Author        | Modifed | Comment           | Reviewer         |
|-------------|---------|---------------|-------|------------|------------------|
| 19-07-2025  |  V1     | Sneha Joshi   | 19-07-2025 | Internal Review   | Siddharth Pawar  |

---

### Table of Contents

1. [Prerequisites](#prerequisites)    
2. [Installation Guide](#installation-guide)  
   - [Check for Existing Installation](#check-for-existing-installation)  
   - [Ubuntu](#ubuntu)  
   - [RedHat](#redhat)  
   - [macOS](#macos)  
   - [Windows](#windows)  
3. [Example JSON Data](#example-json-data)  
4. [Basic Usage](#basic-usage)  
5. [Advanced Queries](#advanced-queries)
6. [Use Cases](#use-cases)
7. [Known Errors in jq Usage](#known-errors-in-jq-usage)
8. [Conclusion](#conclusion)  
9. [Contact Information](#contact-information)  
10. [References](#references)  

---

## Introduction

This documentation outlines practical value of the jq command-line tool, highlighting why it was created, what it offers, and how its features align with modern data processing and automation requirements in software development.

---


## Prerequisites

- Basic understanding of JSON format  
- Terminal or Command Line Interface access  
- Internet access for installation (if not already installed)

---

## Installation Guide

### Check for Existing Installation

Before installing, check if jq is already present:

```bash
jq --version
```
![image](https://github.com/user-attachments/assets/ba0e1885-a62b-4aac-8292-5bec740402c5)

If this command returns a version (e.g., jq-1.6), you already have it installed.

### Ubuntu
> Update your system 

```bash
sudo apt install jq -y
```

![image](https://github.com/user-attachments/assets/a03f59b7-09cb-4c40-9a96-d897e00845cb)


![image](https://github.com/user-attachments/assets/2cfce514-70b5-426e-8a2f-8951b4a88035)

### RedHat

```bash
sudo yum install epel-release -y
sudo yum install jq -y
```

### macOS

```bash
brew install jq
```

### Windows

- Download the .exe file from the [official GitHub releases](https://github.com/stedolan/jq/releases)
- Add the directory containing the .exe to your system’s PATH environment variable

---

## Example JSON Data

Save the following content in a file called data.json for practice:

json
{
  "user": {
    "name": "Alice",
    "roles": ["admin", "editor"],
    "age": 30,
    "address": {
      "city": "New York",
      "zip": "10001"
    }
  },
  "items": [
    {"id": 1, "name": "Item One"},
    {"id": 2, "name": "Item Two"}
  ]
}

![image](https://github.com/user-attachments/assets/6ce30a9a-3f01-4313-aed0-eb9425f2e46a)

---

## Basic Usage

### View Entire JSON

bash
cat data.json | jq '.'

# Prints the entire contents of the JSON file in a human-readable and color-formatted layout.

![image](https://github.com/user-attachments/assets/559b24f4-8ff8-4580-b58f-d5193974c0cd)

### Filter a Field

bash
jq '.user.name' data.json

# Extracts and displays the value of the name field nested inside the user object.

![image](https://github.com/user-attachments/assets/9b28d42b-3a56-4f42-8177-facc26df5ec3)

---

## Advanced Queries

### Access Nested Values

bash
jq '.user.address.city' data.json

# Accesses the nested city field inside address, which is itself a key inside the user object.

![image](https://github.com/user-attachments/assets/38aa411d-b038-480c-9c99-9693025cfbf5)

### Loop Over Arrays

bash
jq '.items[] | .name' data.json

# Iterates over each object in the items array and returns the value of the name key for each item.

![image](https://github.com/user-attachments/assets/390601ae-ae3c-4f76-b494-99580d00cabc)


### Search by Index Value

#### Query:

bash
jq '.user.roles[0]' data.json

# Retrieves the first element (index 0) from the roles array within the user object.

![image](https://github.com/user-attachments/assets/7b319199-c581-4c54-aee2-014827748901)


---

## Use cases
| Scenario | How jq Helps |
|----------|---------------|
| Log Analysis in DevOps Pipelines | Quickly extract and format key data from JSON-formatted logs or API responses for monitoring tools. |
| REST API Response Parsing | Process JSON outputs from curl or other HTTP clients to extract relevant fields programmatically. |
| CI/CD Workflow Automation | Use in scripts to parse build metadata, environment variables, or deployment status in JSON format. |
| Configuration File Manipulation | Read or validate JSON-based config files used by tools like Docker Compose, Terraform, or Kubernetes. |
| Database Migration or Sync Scripts | Parse and transform JSON data exported from NoSQL databases like MongoDB for further automation. |
| Security Auditing | Filter large JSON outputs from security tools (e.g., AWS Inspector, audit logs) to extract vulnerabilities. |
| Cloud Resource Management | Process JSON responses from AWS CLI, Azure CLI, or GCP CLI to summarize or report resource states.|
| Data Transformation in ETL Pipelines | Clean, reshape, or enrich incoming JSON data as part of Extract-Transform-Load workflows. |
| Debugging Microservices | Decode structured log files or API payloads from microservices communicating in JSON. |
| Infrastructure as Code (IaC) Audits | Validate and inspect JSON-based state files from Terraform or CloudFormation. |

---

## Known Errors in jq Usage

This section outlines common issues users may encounter when using jq and how to resolve them.

---

### 1. File Not Found

bash
jq '.' data.json

jq: error: Could not open file data.json: No such file or directory


*Cause:* File data.json doesn't exist in the current directory. 
*Fix:* Ensure the file is present and the path is correct.

---

### 2. Invalid JSON Format


cat broken.json | jq '.'

parse error: Unfinished JSON...


*Cause:* The JSON file is not properly formatted (e.g., missing commas, brackets, quotes).  
*Fix:* Validate the JSON using a linter or online JSON validator before using jq.

---

### 3. Null Output


jq '.user.phone' data.json

null


*Cause:* The queried field does not exist in the JSON.  
*Fix:* Double-check the field name and structure. Use jq 'keys' or jq '.' to inspect structure first.

---

### 4. Index Out of Range


jq '.user.roles[5]' data.json

null


*Cause:* You're accessing an index that doesn’t exist in the array.  
*Fix:* Use jq '.user.roles | length' to verify array size before indexing.

---

### 5. Permission Denied 


jq '.' /root/data.json

jq: error: Could not open file /root/data.json: Permission denied


*Cause:* File is not readable by the current user.  
*Fix:* Run with elevated permissions (e.g., sudo) or change file permissions.

---

## Conclusion 

jq bridges the gap between human-readable JSON and machine-level automation needs—allowing for cleaner, scalable, and more maintainable workflows.

---
## Contact Information

| Name         | Email Address                                 |
|--------------|-----------------------------------------------|
| Sneha Joshi  | sneha.joshi.snaatak@mygurukulam.co            |

---

## References
| *Title*                        | *Link*                                        |
|----------------------------------|-------------------------------------------------|
| Official jq Manual | [Visit](https://stedolan.github.io/jq/manual/)  |
| jq GitHub Releases | [Visit](https://github.com/stedolan/jq/releases)  |


---
title: "Optimized Documentation Workflow"
subtitle: "Using Wiki.js, OpenAPI, and Inline Documentation"
author: "Your Name"
date: "Presentation Date"
theme: "white"
revealOptions:
  transition: "fade"
---

## Introduction

### Importance of Documentation
- Knowledge Sharing
- Onboarding
- Reference Material

### Challenges
- Outdated documentation
- Inconsistent updates

### Solution
- Automated, consistent, and accurate documentation

---

## Overview

### Workflow Overview
1. **Generate Documentation:**
   - Inline code documentation (jsdoc, pdoc, roxygen, doxygen)
   - OpenAPI for REST APIs
2. **Update Wiki.js:**
   - Automate Wiki.js updates through CI/CD
   - Minimize manual updates

---

## Inline Code Documentation

### JavaScript (jsdoc)
- Command: `jsdoc -c jsdoc.json`
- Example Configuration:
  ```json
  {
    "source": {
      "include": ["src"]
    },
    "opts": {
      "destination": "./out/jsdoc"
    }
  }
  ```

### Python (pdoc)
- Command: `pdoc --html --output-dir ./out/pdoc my_python_package`

### R (roxygen2)
- Command:
  ```r
  roxygen2::roxygenise("my_r_package")
  ```

### SQL (doxygen)
- Command: `doxygen -g Doxyfile`
- Example Configuration:
  ```makefile
  INPUT = src/sql_files
  OUTPUT_DIRECTORY = ./out/doxygen
  ```

---

## OpenAPI Documentation

### Purpose
- Standardized API documentation
- Easier integration with tools

### Tools
- `redoc-cli` to generate HTML documentation
- Command: `redoc-cli bundle openapi.yaml --output ./out/openapi/index.html`

### Integration
- **Swagger UI:** Embed via iframes in Wiki.js

---

## Automated Documentation Updates

### GitHub Actions Workflow
- Automated generation of documentation
- GraphQL API to update Wiki.js

### Example Workflow
```yaml
name: Update Documentation

on:
  push:
    branches:
      - main

jobs:
  generate-docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Node.js
        uses: actions/setup-node@v2
      - name: Set up Python
        uses: actions/setup-python@v2
      - name: Install Documentation Generators
        run: |
          npm install -g jsdoc redoc-cli
          pip install pdoc
          apt-get install -y doxygen
      - name: Generate JavaScript Docs
        run: jsdoc -c jsdoc.json
      - name: Generate Python Docs
        run: pdoc --html --output-dir ./out/pdoc my_python_package
      - name: Generate R Docs
        run: |
          Rscript -e "roxygen2::roxygenise('my_r_package')"
          mv my_r_package/man ./out/roxygen
      - name: Generate SQL Docs
        run: doxygen Doxyfile
      - name: Generate OpenAPI Docs
        run: redoc-cli bundle openapi.yaml --output ./out/openapi/index.html
      - name: Upload to Wiki.js
        env:
          WIKIJS_TOKEN: ${{ secrets.WIKIJS_TOKEN }}
          WIKIJS_URL: ${{ secrets.WIKIJS_URL }}
        run: |
          # Replace with appropriate GraphQL mutation
          curl -X POST "${{ secrets.WIKIJS_URL }}/graphql" -H "Authorization: Bearer $WIKIJS_TOKEN" \
            -F "query={ query... }"
```

---

## Wiki.js Automation

### GraphQL API Example
```graphql
mutation {
  pages {
    create(input: {
      path: "/docs/jsdoc/index",
      title: "JavaScript Documentation",
      description: "Auto-generated documentation using jsdoc",
      content: "HTML_CONTENT_HERE",
      isPrivate: false
    }) {
      path
    }
  }
}
```

### Automated Process
- Generate documentation files
- Upload/update in Wiki.js via GraphQL API

---

## CI/CD Pipeline Structure

### Step 1: Generate Documentation
- Inline tools (jsdoc, pdoc, roxygen, doxygen)
- OpenAPI standards

### Step 2: Upload to Wiki.js
- Use GraphQL API to update pages

---

## Conclusion

- Automated, consistent, and accurate documentation
- Efficient workflows using CI/CD and Wiki.js
- Easily maintainable and up-to-date documentation

---

## Q&A

**Questions & Answers**

Invite questions and feedback



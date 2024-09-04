# Building a GitHub Actions Workflow to Generate SBOMs for a JavaScript Project and Docker Image

In this tutorial, you will create a GitHub Actions workflow that:
- Analyzes a JavaScript project to generate a **Software Bill of Materials (SBOM)**.
- Builds a **Docker image** for the project.
- Generates an SBOM for the Docker image, including the Node.js runtime.

This tutorial uses **CycloneDX** to generate the SBOM for the JavaScript project and **Anchore's SBOM Action** to generate the SBOM for the Docker image.

## Prerequisites

Before proceeding, ensure you have the following tools:
- **GitHub repository** containing a JavaScript project with a `package.json` file.
- **Dockerfile** for building the project.
- **Node.js** and **NPM** installed in your project.

## Step 1: Project Structure

Here is the basic structure of your project directory:

```
.
├── .github
│   └── workflows
│       └── sbom-docker.yml  # GitHub Actions workflow file
├── Dockerfile                # Dockerfile to build your app
├── package.json              # JavaScript dependencies
└── index.js                  # Your application code
```

Add the package.json:

```js
{
    "name": "simple-webserver",
    "version": "1.0.0",
    "description": "A simple web server with outdated dependencies",
    "main": "index.js",
    "scripts": {
      "start": "node index.js"
    },
    "dependencies": {
      "express": "4.16.4",
      "lodash": "4.17.20"
    },
    "engines": {
      "node": ">=10.0.0"
    },
    "author": "Your Name",
    "license": "MIT"
  }
```

Add the index.js:

```js
// index.js
const express = require('express');
const _ = require('lodash');

const app = express();
const port = 3000;

// A simple route that responds with "Hello, World!"
app.get('/', (req, res) => {
  res.send('Hello, World!');
});

// Example of using lodash (an outdated dependency)
app.get('/lodash-example', (req, res) => {
  const numbers = [1, 2, 3, 4, 5];
  const doubled = _.map(numbers, num => num * 2);
  res.send(`Doubled Numbers: ${doubled}`);
});

// Start the web server
app.listen(port, () => {
  console.log(`Server is running on http://localhost:${port}`);
});
```

## Step 2: Dockerfile Example

Here’s a simple `Dockerfile` that builds a Node.js project:

```Dockerfile
# Use official Node.js runtime as a parent image
FROM node:16-alpine

# Set working directory
WORKDIR /app

# Copy package.json and install dependencies
COPY package.json ./
RUN npm install

# Copy the rest of the application code
COPY . .

# Start the application
CMD ["node", "index.js"]
```

This Dockerfile installs dependencies from the `package.json` file and runs the application using `index.js`.

## Step 3: GitHub Actions Workflow Using Anchore SBOM Action

Create a GitHub Actions workflow file at `.github/workflows/sbom-docker.yml`:

```yaml
name: Build and Analyze SBOM

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  generate-sbom:
    name: Generate SBOM and Build Docker Image
    runs-on: ubuntu-latest

    steps:
    # Step 1: Checkout the code
    - name: Checkout code
      uses: actions/checkout@v3

    # Step 2: Set up Node.js
    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '16'

    # Step 3: Install dependencies
    - name: Install NPM dependencies
      run: npm install

    # Step 6: Generate SBOM for Javascript using Anchore's SBOM Action
    - name: Generate SBOM for Docker image
      uses: anchore/sbom-action@v0
      with:
        format: json
        artifact-name: sbom-node-js.json

    # Step 5: Build Docker image
    - name: Build Docker Image
      run: |
        docker build -t my-node-app:latest .

    # Step 6: Generate SBOM for Docker image using Anchore's SBOM Action
    - name: Generate SBOM for Docker image
      uses: anchore/sbom-action@v0
      with:
        image: my-node-app:latest
        format: json
        artifact-name: sbom-docker-image.json

    # Step 7: Upload SBOMs as artifacts
    - name: Upload SBOMs
      uses: actions/upload-artifact@v3
      with:
        name: sbom-artifacts
        path: |
          sbom-node-js.json
          sbom-docker-image.json
```

### **Explanation of Workflow Steps**

1. **Checkout code**: This step checks out the repository’s code so that the workflow can access it.
2. **Set up Node.js**: This action installs Node.js (version 16) in the workflow environment.
3. **Install NPM dependencies**: Installs the required dependencies listed in `package.json` using `npm install`.
4. **Build Docker image**: Builds a Docker image using the `Dockerfile` and tags it as `my-node-app:latest`.
5. **Generate SBOM for Docker image**: Uses the official Anchore SBOM Action (`anchore/sbom-action`) to generate an SBOM for the Docker image, including all the components and the Node.js runtime. The SBOM is saved as `sbom-docker-image.json`.
6. **Upload SBOMs as artifacts**: This step uploads the generated SBOMs as artifacts so they can be downloaded and reviewed later.

## Step 4: Running the Workflow

Once the workflow is configured, it will automatically run when:
- You push changes to the `main` branch.
- You open or update a pull request targeting the `main` branch.

### SBOM Output Files:
- **sbom-js-project.json**: Contains the SBOM for the JavaScript dependencies.
- **sbom-docker-image.json**: Contains the SBOM for the Docker image, including the Node.js runtime and other system components.

## Step 5: Accessing the SBOM Artifacts

After the workflow runs successfully, you can download the SBOMs from the workflow run’s **Artifacts** section on the GitHub Actions page.

---

## Conclusion

By setting up this GitHub Actions workflow, you’ve automated the process of:
- Generating an SBOM for your JavaScript project using Anchore.
- Building a Docker image.
- Generating an SBOM for the Docker image (including the Node.js runtime) using Anchore’s SBOM Action.

This setup helps ensure that your project follows best practices for software supply chain security by maintaining transparency and traceability of all components used in your software.

For more information about Anchore's SBOM Action, visit the [Anchore SBOM Action GitHub repository](https://github.com/anchore/sbom-action).

---

### References
- [Anchore SBOM Action](https://github.com/anchore/sbom-action)
- [Docker Documentation](https://docs.docker.com/)

# Static Code Analysis with Sonar (1 hour)

The very first layer of security and quality assurance in any application lies within its source code. Writing functional code is not enough—developers must ensure that their codebase remains secure, reliable, and maintainable over time.

This is where SonarQube comes in. SonarQube is a widely adopted static code analysis tool that inspects your code for bugs, vulnerabilities, code smells, and maintainability issues. 

By scanning your repository, it provides actionable insights to improve code quality, reduce security risks, and enforce consistent coding standards.

To make the process seamless, SonarQube offers a GitHub Action that integrates directly into your continuous integration (CI) pipeline. 

By running this action, your source code is analyzed automatically whenever you push changes, ensuring that issues are caught early—before any resources are deployed to AWS or production environments.

For this tutorial, you’ll use SonarCloud (the hosted version of SonarQube) to set up scanning. After creating an account on sonarcloud.io, you will integrate the GitHub Action into your workflow. Once configured, every build will be validated through Sonar before proceeding, and you’ll be responsible for addressing any issues flagged by the scanner.

The first thing you need to scan in your application is your actual source code.

One very famous tool to do so is **Sonarqube**. There is already a Github Action available for this tool [here](https://github.com/SonarSource/sonarcloud-github-action
).

For this tutorial, you need to create an account on [sonarcloud.io](https://sonarcloud.io).

1. Create a SonarCloud Account
    1. Go to sonarcloud.io.
    1. Sign up with your GitHub account to enable direct integration.
    1. Create a new project in SonarCloud linked to your GitHub repository.
1. Generate a Token for Authentication
    1. In SonarCloud, go to My Account → Security → Generate Tokens.
    1. Copy the generated token (this will serve as your authentication key).
    1. In your GitHub repository, add this token as a secret under:
    
            Settings → Secrets and variables → Actions → New repository secret
    Name it SONAR_TOKEN.



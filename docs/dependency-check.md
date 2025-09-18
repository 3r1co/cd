# Static Code Analysis with OWASP Dependency Check (1 hour)

After analyzing your own source code with SonarQube, the next critical step is to examine the dependencies your application relies on. 

Even if your code is secure, vulnerabilities can be introduced through third-party libraries, frameworks, and packages that your project consumes.

To address this, one of the most widely used tools is the OWASP Dependency Check. Developed under the Open Web Application Security Project (OWASP), it is a Software Composition Analysis (SCA) tool that scans the libraries in your project and identifies known vulnerabilities. 

It does this by comparing your dependencies against the National Vulnerability Database (NVD) and other security advisories, flagging any components with reported CVEs (Common Vulnerabilities and Exposures).

By integrating OWASP Dependency Check into your workflow, you ensure that insecure dependencies are identified early, preventing them from reaching production. This is crucial, as many high-profile breaches have been traced back to outdated or vulnerable third-party components.

Fortunately, OWASP Dependency Check also provides a ready-to-use GitHub Action, which you can find [here](https://github.com/Sburris/dependency-check-action).

In this exercise, integrate this program into your Github workflow.

You need to save your report with the following instruction:

    - name: Archive dependency check reports
      uses: actions/upload-artifact@v1
      with:
        name: reports
        path: reports

Attention: You will have to install the Node.JS dependencies before running the actual dependency check. You can do this by adding the following lines to your workflow after the checkout action:

    - uses: actions/checkout@v2
    - name: Install NPM dependencies
      run: |
        npm install --production --unsafe-perm

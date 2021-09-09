# Docker Vulnerability Scanning with Trivy (1 hour)

So far, we uncovered vulnerabilities in

- your application source
- third party libraries in your application
- your infrastructure as code

Another vulnerability factor can be the libraries that are integrated in your operating system, or in the case of Docker, your Docker image.

A famous open source vulnerability scanner for Docker images is [Trivy](https://github.com/aquasecurity/trivy-action).

Here a way to run the Trivy scan:

    - name: Run Trivy vulnerability scanner
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: 'docker.io/<your-username>/<your-image>:${{ github.sha }}'
          format: 'table'
          exit-code: '1'
          ignore-unfixed: true
          vuln-type: 'os,library'
          severity: 'CRITICAL,HIGH'

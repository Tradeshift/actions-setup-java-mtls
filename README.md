# Actions maven

Setups certs to access private maven repos to support MTLS

## Usage in the workflow

```
name: JAVA-PR
on: [pull_request]
jobs:

  build:

    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Set up JDK 10
        uses: actions/setup-java@v1
        with:
          java-version: 10

      - name: Configure maven
        uses: tradeshift/actions-maven@v1
        with:
          maven-settings: ${{ secrets.MAVEN_SETTINGS }}
          maven-security-settings: ${{ secrets.MAVEN_SECURITY }}
          maven-p12-keystore: ${{ secrets.MAVEN_P12 }}
          maven-p12-keystore-password: ${{ secrets.MAVEN_P12_PASSWORD }}
          company-rootca: ${{ secrets.CACERT }}

      - name: Build with Maven
        run: |
          mvn -Dspring.profiles.active=local clean package -DskipTests

```

Pay attention to the "Configure maven" step. All input parameters are mandatory.

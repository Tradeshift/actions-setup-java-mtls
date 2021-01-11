# Actions maven

Setups certs to access private maven repos to support MTLS

## Usage in the workflow java maven action

Ex: java11 maven 3.6

```
name: JAVA-PR
on: [pull_request]
jobs:

  build:

    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Run maven cmd in specified environmment
        uses: tradeshift/actions-maven/java11-mvn-3-6@master
        with:
          run: |
            mvn -Dspring.profiles.active=local clean package -DskipTests
        env:
          MAVEN_SETTINGS_B64: ${{ secrets.MAVEN_SETTINGS }}
          MAVEN_SETTINGS_SECURITY_B64: ${{ secrets.MAVEN_SECURITY }}
          MAVEN_P12_B64: ${{ secrets.MAVEN_P12 }}
          MAVEN_P12_PASSWORD: ${{ secrets.MAVEN_P12_PASSWORD }}
          ROOTCA_B64: ${{ secrets.CACERT }}

```

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

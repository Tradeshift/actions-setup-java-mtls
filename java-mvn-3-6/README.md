# Actions maven

Setups certs to access private maven repos to support MTLS maven access
Support different Java versions

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
        uses: tradeshift/actions-maven/java-mvn-3-6@master
        with:
          java: 10
          docker-parameters: "-e MY_CUSTOM_ENV=customVal"
          run: |
            mvn -Dspring.profiles.active=local clean package -DskipTests
        env:
          MAVEN_SETTINGS_B64: ${{ secrets.MAVEN_SETTINGS }}
          MAVEN_SETTINGS_SECURITY_B64: ${{ secrets.MAVEN_SECURITY }}
          MAVEN_P12_B64: ${{ secrets.MAVEN_P12 }}
          MAVEN_P12_PASSWORD: ${{ secrets.MAVEN_P12_PASSWORD }}
          ROOTCA_B64: ${{ secrets.MTLS_CACERT }}
          GCLOUD_SERVICE_ACCOUNT_KEY: ${{ secrets.GCLOUD_SERVICE_ACCOUNT_KEY }}

```

## Usage in the workflow sonar maven action

This setup runs maven sonar scanner on the projects

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
        uses: tradeshift/actions-maven/java-mvn-3-6@master
        with:
          java: 10
          docker-parameters: "-e MY_CUSTOM_ENV=customVal"
        env:
          MAVEN_SETTINGS_B64: ${{ secrets.MAVEN_SETTINGS }}
          MAVEN_SETTINGS_SECURITY_B64: ${{ secrets.MAVEN_SECURITY }}
          MAVEN_P12_B64: ${{ secrets.MAVEN_P12 }}
          MAVEN_P12_PASSWORD: ${{ secrets.MAVEN_P12_PASSWORD }}
          ROOTCA_B64: ${{ secrets.MTLS_CACERT }}
          GCLOUD_SERVICE_ACCOUNT_KEY: ${{ secrets.GCLOUD_SERVICE_ACCOUNT_KEY }}
          MTLS_CERT_B64: ${{ secrets.MTLS_CERT }}
          MTLS_KEY_B64: ${{ secrets.MTLS_KEY }}
          SONAR_HOST: "https://sonar.something"
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}

```

## Action parameters

- *java*, this parameter represents java version that is going to be used in the action
- *run*, this parameter represents script that is going to be run after maven cert setup and before sonar run (if sonar is turned on). Be aware the you need add escape symbols for quotes
like ```run: |
    echo \"something\"```
- *docker-parameters*, these is a string containing additional parameters to the docker container running this action

## Action env vars

- MAVEN_SETTINGS_B64
- MAVEN_SETTINGS_SECURITY_B64
- MAVEN_P12_B64
- MAVEN_P12_PASSWORD
- ROOTCA_B64
- GCLOUD_SERVICE_ACCOUNT_KEY
- MTLS_CERT_B64
- MTLS_KEY_B64
- SONAR_HOST
- SONAR_TOKEN
- SONAR_CUSTOM_OPTS

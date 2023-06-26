# Action: Setup Java MTLS

Configure certs to access private maven repos to support MTLS.

# Usage 

```
  - uses: tradeshift/actions-setup-java-mtls@v1
    with:
      java-version: 11
      maven-settings: ${{ secrets.MAVEN_SETTINGS_GH_PG }}
      maven-settings: ${{ secrets.MAVEN_SECURITY }}
      maven-p12: ${{ secrets.MAVEN_P12 }}
      maven-p12-password: ${{ secrets.MAVEN_P12_PASSWORD }}
      mtls-cacert: ${{ secrets.MTLS_CACERT }}
```

## Usage in a full workflow 
Below you have an example of how to use this action in a realistic scenario, together with the `actions/java` and `tradeshift/actions-setup-maven` actions, which sets up `JDK` and `Maven`, respectively.
```
name: JAVA-PR
on: [pull_request]
jobs:
  build:
    runs-on: [self-hosted,ts-large-x64-docker-large]
    steps:
      - uses: actions/checkout@v3
      - id: setupJava
        uses: actions/setup-java@v3
        with:
          java-version: 11
          distribution: 'zulu'
      - name: Set up Maven
        uses: tradeshift/actions-setup-maven@v4.4
        with:
          maven-version: 3.6.3
      - name: Configure maven
        uses: tradeshift/actions-setup-java-mtls@v1
        with:
          java-version: "${{ steps.setupJava.outputs.version }}"
          maven-settings: ${{ secrets.MAVEN_SETTINGS_GH_PG }}
          maven-security: ${{ secrets.MAVEN_SECURITY }}
          maven-p12: ${{ secrets.MAVEN_P12 }}
          maven-p12-password: ${{ secrets.MAVEN_P12_PASSWORD }}
          mtls-cacert: ${{ secrets.MTLS_CACERT }}
      - name: Compile
        run: |
          mvn -B compile
      - name: Test
        run: |
          mvn -B -Dmaven.test.failure.ignore=true install javadoc:javadoc

```
Test change (dont merge)

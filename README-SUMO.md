# Sumo fork of commons-configuration 1.x

Branch `sumo`, based on upstream tag `CONFIGURATION_1_10`. Each release is a tag on `sumo`.

| version | Artifactory | change |
|---|---|---|
| `1.10-Sumo` | `libs-release-local` (hand upload) | pom drops optional `log4j`; classes identical to upstream 1.10 |
| `1.11-Sumo` | `sumo-release-local` | `commons-lang` 2.6 -> `commons-lang3`; lang 2.6 `text.Str*` vendored in `org.apache.commons.configuration.vendor.lang.text`; Java 17 |

## Publish

Uses `artifactory_contextUrl`/`artifactory_password`, the same values as the monorepo's gradle properties.

```sh
export JAVA_HOME=~/.sdkman/candidates/java/17.0.16-amzn   # `/usr/libexec/java_home -v 17` may return 8
export ARTIFACTORY_PASSWORD=<artifactory_password>
mvn -s .mvn/sumo-settings.xml -DskipTests \
  -DaltDeploymentRepository=sumo-artifactory::default::<artifactory_contextUrl>/sumo-release-local \
  source:jar deploy
```

- bump `<version>` first; Artifactory rejects redeploying a release version
- tag the published commit with the version (`git tag 1.11-Sumo`)
- dry run: replace the repository url with `file:///tmp/deploytest`
- `mvn clean test` on JDK 17: 12 failures (`TestXMLConfiguration` x10, `TestFileConfiguration.testSaveInvalidURL*`) are upstream/JDK 17 and pre-existing on `CONFIGURATION_1_10`

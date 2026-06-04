# niord-josm-seachart

The `niord-josm-seachart` library is a copy of the JOSM seachart plugin
([openstreetmap/josm-plugins](https://github.com/openstreetmap/josm-plugins)).

In Niord, the library is used for creating those pesky AtoN icons.

This repository extracts the module from the
[niord mono-repo](https://github.com/NiordOrg/niord/tree/master/niord-josm-seachart)
into a standalone, rarely-changing project so it can be published and consumed as a versioned
[JitPack](https://jitpack.io) artifact instead of being rebuilt on every Niord build.

## Modifications

* The library has been refactored to build using Maven.
* The following JOSM seachart sub-packages have selectively been merged into `niord-josm-seachart`:
  `render`, `s57` and `symbols`.
* To allow for better integration into Niord, `System.exit()` calls and `System.err` logging have
  been purged — a `SeachartException` is thrown instead.

> **Licensing:** the `render`, `s57` and `symbols` sources are derived from the GPLv3-licensed JOSM
> seachart plugin (Copyright Malcolm Herring), so the combined published artifact is **GPL-3.0-only**.
> The only Niord-authored file, `SeachartException.java`, is additionally available under Apache-2.0
> (Copyright Danish Maritime Authority). See the [License](#license) section below.

## Usage (JitPack)

Add the JitPack repository and the dependency to your `pom.xml`:

```xml
<repositories>
    <repository>
        <id>jitpack.io</id>
        <url>https://jitpack.io</url>
    </repository>
</repositories>

<dependency>
    <groupId>com.github.NiordOrg</groupId>
    <artifactId>josm-seachart</artifactId>
    <version>0.1.0</version>
</dependency>
```

> The JitPack coordinates use the GitHub org/repo name (`com.github.NiordOrg:josm-seachart`), not the
> Maven `groupId`/`artifactId` declared in this project's `pom.xml`.

Browse builds at: https://jitpack.io/#NiordOrg/josm-seachart

## Building locally

Requires JDK 21 and Maven 3.6.3+:

```bash
mvn clean install
```

## Release process

JitPack builds artifacts on demand from Git tags, so a release is simply a tag whose name matches the
project version.

1. Bump `<version>` in `pom.xml` (e.g. `0.1.0`).
2. Commit and push to `main`:
   ```bash
   git add . && git commit -m "Prepare release 0.1.0" && git push origin main
   ```
3. Create and push a matching tag:
   ```bash
   git tag 0.1.0
   git push origin 0.1.0
   ```
4. Trigger/verify the build at https://jitpack.io/#NiordOrg/josm-seachart (the first request for a new
   tag kicks off the build; the log is at
   `https://jitpack.io/com/github/NiordOrg/josm-seachart/0.1.0/build.log`).

### JitPack configuration

The build is configured by [`jitpack.yml`](jitpack.yml):

```yaml
jdk:
  - openjdk21
install:
  - mvn wrapper:wrapper -Dmaven=3.6.3
  - ./mvnw install -DskipTests
```

This ensures JitPack uses OpenJDK 21 and Maven 3.6.3 (required for `maven-compiler-plugin` 3.13.0).

## Credits

The main contributor to the seachart JOSM plugin seems to be
[Malcolm Herring](https://github.com/malcolmh).

## License

This project is distributed under the [GNU General Public License v3.0 only](LICENSE).

The bulk of the code (the `render`, `s57` and `symbols` packages) originates from the JOSM seachart
plugin by [Malcolm Herring](https://github.com/malcolmh) and carries GPLv3 file headers; the combined
artifact is therefore GPL-3.0-only. The single Niord-authored file,
`src/main/java/org/niord/josm/seachart/SeachartException.java`, is additionally available under the
Apache License 2.0 (Copyright Danish Maritime Authority).

# Known Issues

> Tracked unresolved items from the DCC-240 fork and migration. Each item includes its risk level, reason it cannot be resolved immediately, and the recommended path forward.

---

## KNOWN-1: Java source packages retain `com.wavesplatform.wavesj.*` naming

**Risk:** LOW (branding only — no security or functionality impact)

**Description:** All Java source packages under `src/main/java/` and `src/test/java/` retain the upstream package namespace `com.wavesplatform.wavesj.*`. Renaming would be a breaking API change for any consumers who import from the old coordinates.

**Why not fixed now:** Breaking change requires a major version bump and migration guide. Deferred to a future sprint.

**Resolution path:** 
- Bump to `2.0.0`
- Rename all source packages to `io.decentralchain.sdk.*`
- Publish migration guide

**Affected files:** All `.java` files under `src/`

---

## KNOWN-2: Runtime dependencies on `com.wavesplatform:*` artifacts

**Risk:** MEDIUM (supply chain — depends on upstream Waves Maven Central artifacts)

**Description:** Two runtime dependencies are published by the Waves team:
- `com.wavesplatform:waves-transactions:1.2.7`
- `com.wavesplatform:lang:1.6.1`

These cannot be easily forked because `lang` is the Ride compiler — a large, complex Scala project (`node-scala/lang`).

**Why not fixed now:** Forking the Ride compiler is a multi-sprint effort tracked as part of the broader node-scala migration.

**Resolution path:**
- Fork `waves-transactions-java` as `io.decentralchain:transactions-java`
- Long-term: build a standalone `io.decentralchain:lang` (Ride compiler) from `node-scala/lang`

**Affected file:** `pom.xml` lines with `com.wavesplatform` dependencies

---

## KNOWN-3: Checkstyle — 3,273 upstream style violations (report-only)

**Risk:** LOW (code quality — no security impact)

**Description:** The upstream WavesJ codebase was not written against Google Java Style. Checkstyle is configured as report-only (`failOnViolation=false`) to avoid blocking CI immediately.

**Why not fixed now:** 3,273 violations require systematic formatting work — a dedicated PR.

**Resolution path:**
- Apply Google Java formatter to all source files in a single formatting commit
- Enable `failOnViolation=true` in `pom.xml`
- Track as `DCC-241`

---

## KNOWN-4: Docker integration test coverage not measured in unit-test-only builds

**Risk:** INFO

**Description:** JaCoCo coverage thresholds are set to 20%/7% for local builds (unit tests only). Docker integration tests (10 test classes) are skipped gracefully when Docker is not available. Full coverage (expected ~70%+) only runs in CI where Docker is available. CI enforces 70%/60% thresholds.

**Resolution path:** No action needed — coverage enforcement is already in `ci.yml`.

---

## KNOWN-5: Transitive compile-scope deps flagged as test-only by dependency analyzer

**Risk:** INFO

**Description:** `mvn dependency:analyze` reports three dependencies as "Non-test scoped test only":
- `org.web3j:crypto` — transitive from `waves-transactions`; used internally by `WavesEthConverter.java` through the upstream library's own API
- `com.wavesplatform:protobuf-schemas` — transitive from `waves-transactions`
- `com.google.protobuf:protobuf-java` — transitive from `waves-transactions`; pinned in `<dependencyManagement>` for version alignment only

These are resolved at compile scope because `waves-transactions` (a runtime compile dep) declares them as compile dependencies. Our production source does not call their APIs directly — but they must remain on the compile classpath at runtime for `waves-transactions` to function.

**Why not fixed now:** Moving them to `test` scope would break `waves-transactions` at runtime (it needs them on the classpath). Fixing this properly requires forking `waves-transactions` (see KNOWN-2).

**Resolution path:** Resolved automatically when KNOWN-2 is addressed (fork of `waves-transactions` → full control over the dependency tree).

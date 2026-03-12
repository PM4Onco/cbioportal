# PM4Onco patch guide for `release6.4.1`

## Scope
This bundle ports PM4Onco customizations from `origin/release6.0.4` onto base `v6.4.1`.

Comparison note:
- `origin/release6.0.4` is far behind `v6.4.1` (many upstream commits missing).
- The PM4Onco-specific delta is the commit set in `v6.4.1..origin/release6.0.4` (3 commits).
- These were grouped into 2 patch groups for this release line.
- One additional compatibility fix was added for `v6.4.1` build/runtime (`mongo-java-driver` and `bson` explicit dependencies).

## Branch created locally
- `release6.4.1`
- Base: `v6.4.1`

## Patch groups
1. `0001-pm4onco-align-dependency-overrides-from-release6.0.4.patch`
- Purpose: align dependency overrides carried by PM4Onco in `pom.xml`.
- Changes:
  - remove `spring-boot-starter-data-mongodb`
  - add `javax.xml.bind:jaxb-api:2.3.1`

2. `0002-pm4onco-remove-genie-performance-workflow.patch`
- Purpose: remove workflow `.github/workflows/genie-performance-test.yml`

3. `0003-fix-build-add-explicit-mongo-driver-dependencies-for.patch`
- Purpose: keep Mongo driver classes available after dependency changes when running on `v6.4.1`.
- Changes:
  - add `org.mongodb:mongo-java-driver:${mongo_java_driver.version}`
  - add `org.mongodb:bson:${mongodb_bson.version}`

## Apply from clean base
From `v6.4.1`:

```bash
git checkout -b release6.4.1 v6.4.1
git am pm4onco/patches/0001-pm4onco-align-dependency-overrides-from-release6.0.4.patch
git am pm4onco/patches/0002-pm4onco-remove-genie-performance-workflow.patch
git am pm4onco/patches/0003-fix-build-add-explicit-mongo-driver-dependencies-for.patch
```

## Verify
```bash
git log --oneline --decorate -n 5
git diff --name-status v6.4.1..HEAD
```

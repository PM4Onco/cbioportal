# PM4Onco patch guide for `release6.4.4`

## Scope
This bundle ports PM4Onco customizations onto base `v6.4.4`.

Comparison note:
- Base is clean upstream tag `v6.4.4`.
- PM4Onco delta for this release line is captured in 3 commits:
  - dependency override alignment in `pom.xml`
  - removal of workflow `.github/workflows/genie-performance-test.yml`
  - explicit Mongo driver dependencies to keep runtime/build compatibility

## Branch created locally
- `release6.4.4`
- Base: `v6.4.4`

## Patch groups
1. `0001-pm4onco-align-dependency-overrides-from-release6.0.4.patch`
- Purpose: align PM4Onco dependency overrides in `pom.xml`.
- Changes:
  - remove `spring-boot-starter-data-mongodb`
  - add `javax.xml.bind:jaxb-api:2.3.1`

2. `0002-pm4onco-remove-genie-performance-workflow.patch`
- Purpose: remove `.github/workflows/genie-performance-test.yml`

3. `0003-fix-build-add-explicit-mongo-driver-dependencies-for.patch`
- Purpose: keep Mongo classes available after dependency override changes.
- Changes:
  - add `org.mongodb:mongo-java-driver:${mongo_java_driver.version}`
  - add `org.mongodb:bson:${mongodb_bson.version}`

## Apply from clean base
From `v6.4.4`:

```bash
git checkout -b release6.4.4 v6.4.4
git am pm4onco/patches/0001-pm4onco-align-dependency-overrides-from-release6.0.4.patch
git am pm4onco/patches/0002-pm4onco-remove-genie-performance-workflow.patch
git am pm4onco/patches/0003-fix-build-add-explicit-mongo-driver-dependencies-for.patch
```

## Verify
```bash
git log --oneline --decorate -n 5
git diff --name-status v6.4.4..HEAD
```

## Notes from review
- Local build verification in this environment is blocked by Maven repository DNS resolution to `build.shibboleth.net` and sandboxed network restrictions.
- No merge conflicts occurred while applying the 3 PM4Onco commits to `v6.4.4`.

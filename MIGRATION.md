# Apple package relocation

Relocation date: 2026-10-16. The owner explicitly requested skipping code review.
No implementation review, source changes or regression-test conversion was
performed. This move preserves the currently maintained bindings byte for byte.

## Source and preservation

Input: NexusFPC revision `b7bb9f717c6baf94eee76c8c051635184112680e`.
Destination: `https://github.com/nxrp-dev/nexus-packages-apple.git`, cloned at
Nexus `packages/nexus-packages-apple`. Source is at the repository root in
`univint`, `cocoaint` and `iosxlocale`, rather than the standard package repository.

| Package | Original files | Uncompressed bytes | Registered unit roots |
| --- | ---: | ---: | ---: |
| univint | 909 | 12,041,534 | 2 |
| cocoaint | 1,731 | 5,022,576 | 59 |
| iosxlocale | 5 | 30,616 | 2 |
| **Total** | **2,645** | **17,094,726** | **63** |

Each package's `migration` directory contains an untouched original-source
archive, its SHA-256, the per-file SHA-256 manifest and the registered root list.
Every copied original file and every archived file was checked against those
hashes before removing the old source. No examples, namespace wrappers or
notices were discarded. Common FPC distribution notices were additionally
copied beside each package. [migration-source.json](migration-source.json)
records the revision, counts and archive digests.

Registered roots and build flags were recovered from the retired recipes at
NexusFPC `781faeb495d6aeb9d98facf441b826958af2d378`. The retired Make/fpmake
infrastructure is not restored. Original Git history remains in NexusFPC;
canonical upstream credit, import-date evidence and per-component licensing
are in [Nexus attributions](../../ATTRIBUTIONS.md#apple-platform-packages).

## Integration

- Added three intrinsic nxpackage descriptors and static-unit nxbuild recipes.
- Added the dedicated repository catalog and shared Apple target environment.
- Registered the repository and all three descriptors in Nexus's machine catalog.
- Added relocated unit paths to the Pascal language server defaults and saved
  IDE search-path inventory.
- Removed all three old package trees from NexusFPC; updated its package
  checklist and historical Apple-maintenance document with their new locations.
- No compiler or RTL source changes are part of the relocation.

## Verification

The new repository catalog, all three package descriptors and Nexus's machine
catalog validate with the existing NexusScript dialect: five successful checks.

The nxbuild planner accepts all 12 applicable package/target combinations:

| CPU / OS | UniversalInterfaces | AppleLocale | CocoaInterfaces |
| --- | --- | --- | --- |
| x86_64 / darwin | planned | planned | planned |
| aarch64 / darwin | planned | planned | planned |
| x86_64 / iphonesim | planned | planned | excluded |
| aarch64 / iphonesim | planned | planned | excluded |
| aarch64 / ios | planned | planned | excluded |

Planning used scratch copies with an existing host compiler path solely to
satisfy the planner's executable-exists check. It verified source paths,
conditional selection, flags, dependencies' unit paths and output ordering;
it did not execute that compiler as an Apple compiler. The tracked environment
retains explicit Apple compiler/RTL settings matching documented Forge outputs.

All 2,645 imported files, original snapshots and archive hashes verify.
The changed language-server search-path unit builds with Nexus nxbuild and
NexusFPC on Linux; the saved IDE inventory remains valid JSON.
[validation.json](validation.json) records the completed checks and limits.
Detailed scratch plans and the consumer build log are under Nexus ignored
`output/apple-package-relocation`.

## Unverified limits

Matching Apple cross-compiler/RTL SDKs are not installed on this Linux host.
Package compilation, Apple framework linking, runtime execution and a full
Apple consumer build were not performed. Configure `Apple.Environment.nxscript`
with matching outputs from NexusFPC's documented Forge cross builds, then run
the recipes as described in [README.md](README.md). No tests were marked passing
on the strength of a plan or a source hash.

Changes remain local; no commit or push was requested.

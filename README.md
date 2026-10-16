# Nexus Apple packages

Dedicated Apple-platform packages relocated from NexusFPC on 2026-10-16:

| Package | Descriptor | Build recipe | Targets |
| --- | --- | --- | --- |
| `univint` | [UniversalInterfaces](univint/Package.nxscript) | [UniversalInterfaces.nxpackage](univint/UniversalInterfaces.nxpackage) | x86_64/aarch64 macOS and simulators; aarch64 iOS |
| `cocoaint` | [CocoaInterfaces](cocoaint/Package.nxscript) | [CocoaInterfaces.nxpackage](cocoaint/CocoaInterfaces.nxpackage) | x86_64/aarch64 macOS |
| `iosxlocale` | [AppleLocale](iosxlocale/Package.nxscript) | [AppleLocale.nxpackage](iosxlocale/AppleLocale.nxpackage) | x86_64/aarch64 macOS and simulators; aarch64 iOS |

The [repository catalog](Packages.RepositoryIndex.nxscript) lists all three.
CocoaInterfaces and AppleLocale directly require UniversalInterfaces.
[Nexus attributions](../../ATTRIBUTIONS.md#apple-platform-packages) is the
canonical upstream credit, import evidence and license record. Original headers,
namespace wrappers and notices remain unchanged. Common FPC distribution notices
are copied beside each package; those notices do not replace individual Apple
interface notices. The repository does not impose one license on mixed material.

## Build in a Nexus workspace

Clone this repository at `Nexus/packages/nexus-packages-apple`. The nxpackage
metadata uses Nexus's caller-owned dialect catalog; the `.nxpackage` build
recipes use the Nexus nxbuild dialect and share
[Apple.Environment.nxscript](Apple.Environment.nxscript).

Configure that file's compiler, matching RTL and LLVM utility paths first.
Its initial paths match the Win64-hosted cross outputs documented in NexusFPC's
`NexusFPC.Forge.md`; change them for another installation or host. The compiler
must target the selected CPU. A Linux RTL cannot substitute for an Apple RTL.
The retained combinations are `x86_64/darwin`, `aarch64/darwin`,
`x86_64/iphonesim`, `aarch64/iphonesim` and `aarch64/ios`.

From the Nexus root, for example:

```sh
nxbuild /action=build /project=packages/nexus-packages-apple/univint/UniversalInterfaces.nxpackage /targetos=darwin /targetcpu=aarch64
nxbuild /action=build /project=packages/nexus-packages-apple/cocoaint/CocoaInterfaces.nxpackage /targetos=darwin /targetcpu=aarch64
nxbuild /action=build /project=packages/nexus-packages-apple/iosxlocale/AppleLocale.nxpackage /targetos=darwin /targetcpu=aarch64
```

Build UniversalInterfaces first. Output is
`Nexus/output/nxbuild-apple/<cpu>-<os>/<package>`; consumers add the appropriate
unit directories. Runtime package SDKs, Apple framework linking and execution
are separate from these static-unit recipes.

UniversalInterfaces compiles the two existing umbrella roots with MacPas.
CocoaInterfaces preserves the 59 roots and macro definitions from its retired
package recipe; Cocoa sources retain their own Free Pascal Objective-C language
mode directives. AppleLocale builds its two ordinary runtime units. Namespaced
wrappers are retained for consumers that compile those separately.

See [MIGRATION.md](MIGRATION.md) for file hashes, completed validation and limits.
The owner explicitly waived code review for this relocation. No source changes,
new review findings, regression fixtures or implementation fixes are included.

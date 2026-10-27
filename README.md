# Dirichlet CocoaPods Specs

Public CocoaPods specifications for the Dirichlet iOS SDKs. No GitHub account or
token is required. Frameworks and resources are downloaded from the existing
Dirichlet CDN; this repository contains metadata only.

## Integration

Put both sources at the top of your Podfile, keeping the Dirichlet source first:

```ruby
source 'https://github.com/dirichlet-ai/dirichlet-specs.git'
source 'https://cdn.cocoapods.org/'

platform :ios, '12.0'
use_frameworks! :linkage => :static

target 'YourApp' do
  pod 'DirichletMediationSDK', '5.3.1.7'
  pod 'DirichletMediationAdapterDRA', '5.3.1.7'
  pod 'DirichletMediationAdapterCSJ', '5.3.1.7'
  pod 'DirichletMediationAdapterGDT', '5.3.1.7'
end
```

For ADN integration, replace those four declarations with
`pod 'DirichletAdSDK', '5.3.1.7'`. Core is resolved transitively. Select one
published SDK version and keep all Dirichlet Pods on that exact version.
Only include adapters needed by your app. Other vendors' dependencies continue
to resolve from the public CocoaPods source.

Run `pod install --repo-update` and open the generated `.xcworkspace`.
For an existing project, preserve `Podfile.lock` and inspect its `SPEC REPOS`
section after installation. Adding this source does not change your locked
SDK version. Do not remove other sources required by unrelated dependencies.

If `SPEC REPOS` still lists Dirichlet Pods under `trunk`, pin your installed
Dirichlet Pods to their existing exact versions, then run a targeted update:

```sh
# ADN example; for mediation include its installed adapters and SDK as well.
pod update DirichletCoreSDK DirichletAdSDK --repo-update
```

Use only the Pod names already installed in your project. Review the lockfile
diff: the source should change while SDK and third-party versions stay fixed.
Do not delete the lockfile or run an unrestricted `pod update` just to migrate.
Avoid adding `:source` to every direct dependency: CocoaPods then ignores global
sources, which can make third-party transitive dependencies unavailable.

Unity packages that include this migration add the sources during iOS export.
For older packages, add the same two lines to the exported Podfile before
running CocoaPods; a later export may overwrite that manual change.

## Publication

The SDK release workflow publishes the six matching specifications in one Git
commit after validating the candidate and dependencies. Existing versions are
immutable: an identical retry is accepted, different content must use a new
version. CocoaPods Trunk does not need to be writable.

`bootstrap-manifest.json` records the 133 public historical specifications
imported from CocoaPods/Specs on 2026-10-27, their original URLs, and the SHA-256
of each imported file. Their versions, dependencies, licenses and CDN URLs are
preserved. New releases are not part of this historical manifest.

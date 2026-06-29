# Vendored NuGet packages (non-nuget.org)

These `.nupkg` files are the **only** build/test dependencies that are not
available on nuget.org. They are committed here so the repo can restore and
test in an offline / network-restricted container without reaching the Azure
`dnceng` feed.

They are referenced as a local package source by the repo-root `NuGet.config`
(`vendored-non-nugetorg`).

## Contents

| Package | Version | Origin feed | Used by |
|---|---|---|---|
| `Microsoft.DotNet.RemoteExecutor` | `10.0.0-beta.25563.105` | Azure dnceng public | `tests/ImageSharp.Tests` |
| `Microsoft.DotNet.XUnitExtensions` | `8.0.0-beta.23580.1`  | Azure dnceng public | `tests/ImageSharp.Tests` |

Their transitive dependencies (`xunit.*`, `Microsoft.Diagnostics.Runtime`,
`System.Runtime.InteropServices.RuntimeInformation`) are all on nuget.org and
are not vendored.

Everything else the build needs (including `SixLabors.Licensing`, `MinVer`,
`Microsoft.SourceLink.GitHub`) is on nuget.org, so it is not vendored here.

## How these were obtained

Versions come from `tests/Directory.Build.targets`. Each was downloaded from
the Azure dnceng public flat-container feed:

```bash
FEED="https://pkgs.dev.azure.com/dnceng/public/_packaging/dotnet-eng/nuget/v3/flat2"
# lowercase id + version
curl -L -o microsoft.dotnet.remoteexecutor.10.0.0-beta.25563.105.nupkg \
  "$FEED/microsoft.dotnet.remoteexecutor/10.0.0-beta.25563.105/microsoft.dotnet.remoteexecutor.10.0.0-beta.25563.105.nupkg"
curl -L -o microsoft.dotnet.xunitextensions.8.0.0-beta.23580.1.nupkg \
  "$FEED/microsoft.dotnet.xunitextensions/8.0.0-beta.23580.1/microsoft.dotnet.xunitextensions.8.0.0-beta.23580.1.nupkg"
```

## Refreshing after a version bump

If the versions in `tests/Directory.Build.targets` change, re-download the
matching `.nupkg` here and delete the stale ones.

# RocketModFix.LDM.Redist

The official **Rocket (Legally Distinct Missile)** assemblies — `Rocket.API.dll`, `Rocket.Core.dll`, `Rocket.Unturned.dll` — packaged for NuGet and refreshed automatically when [SmartlyDressedGames/Legally-Distinct-Missile][ldm] ships a new build with the Unturned dedicated server. Reference a package instead of copying DLLs out of the server's `Extras/Rocket.Unturned` folder after every update.

[![NuGet](https://img.shields.io/nuget/v/RocketModFix.LDM.Redist?label=RocketModFix.LDM.Redist)](https://www.nuget.org/packages/RocketModFix.LDM.Redist)

```
dotnet add package RocketModFix.LDM.Redist
```

## Rocket-only — bring your own Unturned redist

This package ships **only** the three Rocket assemblies and declares **no dependencies**. Rocket's API exposes Unturned and Unity types, so you also need an Unturned redist (for `Assembly-CSharp`, `Steamworks.NET`, `SDG.NetTransport`) and a UnityEngine redist alongside it — pick whichever variant suits you (Server, Client, `.Publicized`, …):

```xml
<ItemGroup>
  <PackageReference Include="RocketModFix.LDM.Redist" Version="*" />
  <!-- Bring your own Unturned + Unity assemblies (any variant you like): -->
  <PackageReference Include="RocketModFix.Unturned.Redist.Server" Version="*" />
  <PackageReference Include="RocketModFix.UnityEngine.Redist" Version="*" />
</ItemGroup>
```

Forget the second group and the compiler tells you exactly what's missing — a clear `CS0012` ("the type … is defined in an assembly that is not referenced") naming the assembly to add a redist for.

Why keep it separate? Game-redist and Rocket-redist are two different things. You already have several Unturned redist flavours (Client/Server, `.Publicized`, prerelease); coupling Rocket to one of them would take that choice away. So this stays independent and you compose the pair you want.

## Versioning

The package version is the **Rocket version** (e.g. `4.9.3.18`), read from `Rocket.Unturned.module` — **not** the Unturned game version. A new version is published only when SDG actually changes Rocket, so most Unturned patches produce no release here.

## How it works

Everything runs on GitHub Actions — no external servers. A scheduled job pulls just the `Extras/Rocket.Unturned` assemblies from the dedicated server's content depot (DepotDownloader, anonymous). When the Rocket version changes it **opens a PR**; `redist-verify` checks the files, SHA-256 hashes and that the version isn't a downgrade; on merge `redist-publish` packs and pushes to **NuGet** (OIDC Trusted Publishing) plus a **GitHub Packages** mirror. No stored API keys.

## License

The redistributed Rocket assemblies are part of the **Legally Distinct Missile**, © 2019 Sven Mawby, maintained by Smartly Dressed Games, under the [MIT License](LICENSE); this repository's packaging is MIT too. This project only redistributes Rocket — it is not otherwise affiliated with or endorsed by Smartly Dressed Games.

[ldm]: https://github.com/SmartlyDressedGames/Legally-Distinct-Missile

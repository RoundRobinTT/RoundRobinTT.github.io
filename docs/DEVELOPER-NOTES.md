## Developer Notes — Local Convenience Copy of Assets and Logos

For local development convenience, you may find it helpful to automate the copying of the
`assets` and `logos` folders from your local clone of the **RoundRobinTT.github.io** repository
(`gh-pages` branch, under `docs/`) into the `RoundRobinSiteOutput` folder inside your local clone
of **WellandValleyCC.github.io**.

This allows the Round Robin site to render correctly when served locally using:

```
dotnet serve -d RoundRobinSiteOutput -o
```

### Optional: Local‑Only Visual Studio Pre‑Build Step

If you are using Visual Studio, you can add a **local‑only** pre‑build step that performs this copy
automatically each time you build the `ClubSiteGenerator` project.

To do this without affecting other contributors, create or edit the file:

```
ClubSiteGenerator.csproj.user
```

This file is not committed to source control and applies only to your machine.

Add the following:

```xml
<Project>
  <PropertyGroup>
    <PreBuildEvent>
      if exist "C:\repos\wvcc\RoundRobinTT.github.io\docs\assets" (
          xcopy /E /I /Y "C:\repos\wvcc\RoundRobinTT.github.io\docs\assets" "$(ProjectDir)..\RoundRobinSiteOutput\assets"
      )
      if exist "C:\repos\wvcc\RoundRobinTT.github.io\docs\logos" (
          xcopy /E /I /Y "C:\repos\wvcc\RoundRobinTT.github.io\docs\logos" "$(ProjectDir)..\RoundRobinSiteOutput\logos"
      )
    </PreBuildEvent>
  </PropertyGroup>
</Project>
```

### Notes

- This step is **entirely optional** and affects only your local environment.
- The authoritative copies of `assets` and `logos` live in the **RoundRobinTT.github.io** repo.
- The copies inside `RoundRobinSiteOutput` are generated and **not tracked** by git.
- If the Round Robin repo is not present on your machine, the build will still succeed; the copy
  step simply does nothing.

This setup keeps the two repositories cleanly decoupled while making local development smooth and predictable.
# Developer Notes

This project uses **two separate GitHub repositories** that work together to generate and publish the Round Robin Time Trial Series website.

Understanding how these repos interact — and how to work with them locally — will make development smooth and predictable.

---

# 📘 Contributor Quickstart — Round Robin & WVCC Repos

The Round Robin site is built from two repositories:

- **RoundRobinTT.github.io**  
  Contains the *actual published site*, served from the `main` branch using the `/docs` folder.  
  This repo holds the authoritative copies of:
  - `docs/assets`
  - `docs/logos`

- **WellandValleyCC.github.io**  
  Contains the **ClubSiteGenerator** and produces the generated output folder  
  `RoundRobinSiteOutput` for local development.

Only the RoundRobin repo contains the real, cloud‑hosted assets.  
The WVCC repo generates the site but does not store assets.

---

## 🗂️ Repository Roles

### **1. RoundRobinTT.github.io (main branch, /docs folder)**

This repo contains:

- The published Round Robin site  
- The authoritative `assets` folder (CSS, future JS, etc.)  
- The authoritative `logos` folder (club logos + Round Robin branding)  
- The Index page and all event/competition pages  
- The READMEs documenting the asset taxonomy

This is the **source of truth** for all Round Robin web content.

### **2. WellandValleyCC.github.io (master branch)**

This repo contains:

- The **ClubSiteGenerator** C# project  
- The build logic that generates the Round Robin site  
- The `RoundRobinSiteOutput` folder (local only, ignored by git)

This repo **does not** store assets or logos — it only generates the site.

---

# 🏗️ Local Development Workflow

To preview the Round Robin site locally, you must generate the full static site into
`RoundRobinSiteOutput`. This is a three‑step process:

1. **Extract event data** from the master spreadsheet  
2. **Process the extracted data** into SQLite DBs
3. **Generate the static site** using the ClubSiteGenerator  

Once the site is generated, you can serve it locally.

---

## 1. Extract Club Events from the Spreadsheet

Run the Python extraction script from the root of the WVCC repo:

```powershell
cd C:\repos\wvcc\WellandValleyCC.github.io\
python3 scripts/extract_club_events.py data/ClubEvents_2026.xlsx data/extracted/
```

This script:

- Reads the master spreadsheet  
- Normalises and validates the event rows  
- Writes CSV files into `data/extracted/`  

These files are consumed by the next step.

---

## 2. Process Extracted Data

Next, run the ClubProcessor to convert the extracted CSV into Round Robin–specific event data in SQLite tables:

```powershell
& "C:\repos\wvcc\WellandValleyCC.github.io\processor\ClubProcessor\bin\Debug\net9.0\ClubProcessor.exe" --mode events --file C:\repos\wvcc\WellandValleyCC.github.io\data\extracted\2026
```

This step:

- Reads the extracted CSVs  
- Applies Round Robin logic  
- Produces processed event data for the site generator  

---

## 3. Generate the Static Site

Finally, run the ClubSiteGenerator:

```powershell
& "C:\repos\wvcc\WellandValleyCC.github.io\processor\ClubSiteGenerator\bin\Debug\net9.0\ClubSiteGenerator.exe" --year 2026
```

This produces the full static site into:

```
RoundRobinSiteOutput/
```

This folder is:

- local only  
- ignored by git  
- safe to delete and regenerate at any time  

---

## 4. Serve the Site Locally

Once `RoundRobinSiteOutput` has been generated, you can preview the site using:

```powershell
dotnet serve -d RoundRobinSiteOutput -o
```

This launches a local web server and opens the site in your browser.

Because `RoundRobinSiteOutput` is **not** version‑controlled, it must contain copies of the
`assets` and `logos` folders from the RoundRobinTT.github.io repo for the site to render correctly.
(See the section on optional automatic copying.)

---

# ⚙️ Optional: Automatic Copying of Assets & Logos (Local Only)

For convenience, you may choose to automate the copying of:

- `RoundRobinTT.github.io/docs/assets`
- `RoundRobinTT.github.io/docs/logos`

into:

- `WellandValleyCC.github.io/RoundRobinSiteOutput`

This ensures your local preview always has the correct styling and branding.

This step is **entirely optional** and affects only your local environment.

---

## Visual Studio Local‑Only Pre‑Build Step

If you use Visual Studio, you can add a pre‑build step that performs this copy automatically each time you build the `ClubSiteGenerator` project.

To do this without affecting other contributors, create or edit:

```
ClubSiteGenerator.csproj.user
```

Add:

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

- The authoritative copies of `assets` and `logos` live in the **RoundRobinTT.github.io** repo.  
- The copies inside `RoundRobinSiteOutput` are generated and **not tracked** by git.  
- If the Round Robin repo is not present on your machine, the build will still succeed; the copy step simply does nothing.

This setup keeps the two repositories cleanly decoupled while making local development smooth and predictable.

---

# 🧭 Summary

- **RoundRobinTT.github.io** holds the real site and all authoritative assets (served from `/docs`).  
- **WellandValleyCC.github.io** generates the site but does not store assets.  
- `RoundRobinSiteOutput` is local‑only and must contain copies of assets/logos.  
- A local‑only pre‑build step can automate this for convenience.  

This structure keeps the repos clean, predictable, and easy to maintain while giving you a smooth development experience.
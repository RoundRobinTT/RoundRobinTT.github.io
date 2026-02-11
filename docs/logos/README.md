# Logos

This directory contains **brand identity assets** used by the Round Robin TT Series and participating clubs. These files are global, stable, and reused across multiple pages of the Round Robin site.

They are **not** page‑specific or decorative images. Only identity assets belong here.

---

## Folder Structure

```
/logos
    hcrc.png
    kcc.png
    lfcc.png
    ratae.png
    rfw.png
    wvcc.png
    /round-robin
        rr-header-2026.png
```

### What each group represents

- **Club logos (top level)**  
  Logos for participating clubs in the Round Robin TT Series.  
  These are used throughout the site wherever club identity is displayed.

- **Round Robin branding (`/round-robin`)**  
  Assets representing the Round Robin TT Series itself.  
  These are global identity files, not tied to any single page.

---

## Source of Truth

Logos exist in **two places**:

1. **Published site source (authoritative copy)**  
   `RoundRobinTT.github.io/docs/logos`  
   This is the version that GitHub Pages serves publicly.

2. **Local development output (generated, ignored by git)**  
   `WellandValleyCC.github.io/RoundRobinSiteOutput/logos`  
   This copy is required so that local development (`dotnet serve`) renders pages correctly.

Because the output folder is ephemeral and excluded from version control, the logos must be **manually copied** into both locations when new assets are added.

This duplication is intentional and pragmatic:  
logo changes are rare, and this avoids brittle cross‑repo dependencies.

---

## Workflow for Adding New Logos

1. Add the new logo to:  
   `RoundRobinTT.github.io/docs/logos`  
   (or `docs/logos/round-robin` if it’s Round Robin branding)

2. Copy the same file into:  
   `WellandValleyCC.github.io/RoundRobinSiteOutput/logos`

3. Run the local server to verify:  
   ```
   dotnet serve -d RoundRobinSiteOutput -o
   ```

4. Commit only the change in the **RoundRobinTT.github.io** repo.  
   The WVCC copy is local‑only and should not be committed.

---

## Naming Conventions

- Use clear, descriptive filenames (e.g., `wvcc.png`, `hcrc.png`).
- For Round Robin branding, use versioned names (e.g., `rr-header-2026.png`) to avoid cache issues.
- Keep all filenames lowercase for consistency.

---

## Guidelines

- Only store **identity** assets here.  
  Do not place photos, decorative images, or content graphics in this folder.

- Keep the structure tidy and intention‑revealing.  
  Future contributors should immediately understand where each type of logo belongs.

- When in doubt:  
  If the image represents a **club** or the **Round Robin brand**, it belongs here.  
  If it represents **content**, it belongs in `/assets`.

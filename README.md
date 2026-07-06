# C-Prolog

[![SWH](https://archive.softwareheritage.org/badge/origin/https://github.com/mathfichen/C-Prolog/)](https://archive.softwareheritage.org/browse/origin/?origin_url=https://github.com/mathfichen/C-Prolog)

[![SWH](https://archive.softwareheritage.org/badge/swh:1:dir:5a74739fa559db09e9a95cb4b25dd4beec2e7328/)](https://archive.softwareheritage.org/swh:1:dir:5a74739fa559db09e9a95cb4b25dd4beec2e7328;origin=https://github.com/mathfichen/C-Prolog;visit=swh:1:snp:96395c1551cabc929796e449845c37ac267f7453;anchor=swh:1:rev:178123d66587e23f3de8f4b0f0570a73904087ef)

This repository contains the archived source code of C-Prolog, developped by Fernando Pereira as of 1982 at EdCAAD, Dept. of Architecture, University of Edinburgh, for a VAX computer.
C-Prolog is based on the Prolog system written in IMP by Luis Damas for ICL 2900 computers, with some contributions by Lawrence Byrd. *

## Main Branch

The main branch contains the original materials as well as the metada linked to C-Prolog. 
The original finds are stored in the Depository containing the raw materials and the browsable source.
- Folder [raw materials](./raw_materials) is for the original source code materials, as they have been found or submitted.
- Folder [source_code](./source_code) is for a machine readable version of the source code. Source files, with the right extension, have to be accessible through the GitHub web interface, e.g., archives should be decompressed, code should be transcribed if provided by images, etc.This folder serves as a base for the reconstruction of the development history as a git repository.
- Folder [metadata](/.metadata) holds various files with meta information to be updated throughout the process. 


## SourceCode Branch
The SourceCode branch contains the reconstructed synthetic development history of C-Prolog. 
Each version of the source code has been stacked one upon the other using successive commits. 


# The process

This repository was created with the support of the Software Heritage Acquisition Process (SWHAP).

For general considerations about the process, check out the [SWHAP process](https://www.softwareheritage.org/swhap/) as initially published in 2019 with UNESCO.
For a detailed step by step description of the process, check out the [SWHAP guide]([https://github.com/mathfichen/swhapguide](https://github.com/SoftwareHeritage/swhapguide/blob/master/SwhapGuide-GitHub-CLI.pdf)). The SWHAP guide is itself a simplified version of the step by step [SHWAP@Pisa guide](https://github.com/SoftwareHeritage/swhapguide/blob/master/SWHAP%40Pisa.pdf) published by the university of Pisa.

C-Prolog has been archived using a partly automated version of the process described in SWHAP guide, based on Guidehub Actions. The automation scripts can be found in the scripts folder. The workflow can be found in .githb. A preliminary documentation of this automation process can be found below. This process is currently being tested and improved, and more detailed documentaiton will be published in the future. 


## SWHAP GitHub-native Template (Wild_Life-style)
This repo is a fully GitHub-based workflow to curate legacy raw materials (archives/dirs), build flattened release trees, and publish a synthetic history on a dedicated `SourceCode` branch — then archive on Software Heritage via “Save Code Now”. No local shell required.
### Key ideas
- Contributors upload files via the GitHub web UI under `raw\\\_materials/<release-id>/...`.
- Maintainers define the canonical release order (and extraction rules) in `metadata/releases.yaml` — array order = commit order.
- CI uses libarchive (`bsdtar`) + p7zip + ncompress to unpack `.zip`, `.tar`, `.tar.gz`, `.tgz`, `.tar.bz2`, `.tar.xz`, `.tar.Z`, `.7z`, and directories.
- Synthetic history is rebuilt deterministically from `metadata/releases.yaml` and the material under `raw\\\_materials/`.

### Repository layout
```
.
├─ raw\\\_materials/                 # Upload raw inputs here (via GitHub UI)
│  └─ <release-id>/              # e.g., life\\\_090, v1.0, 1991-05-12
├─ source\\\_code/                   # (generated) flattened trees for each release
├─ metadata/
│  ├─ releases.yaml              # 👈 authoritative ordered manifest
│  ├─ checksums.csv              # computed on CI
│  └─ journal.md                 # curation log (CI appends)
├─ scripts/                       # automation (Python)
│  ├─ extract\\\_any.py
│  ├─ build\\\_source\\\_tree.py
│  ├─ make\\\_synthetic\\\_history.py
│  └─ update\\\_metadata.py
└─ .github/workflows/             # GitHub Actions
   ├─ validate-and-dryrun.yml
   ├─ curate-pr.yml
   ├─ regenerate-sourcecode.yml
   └─ archive-swh.yml
```
### One-time initialization (after you import/copy this template)
- Commit baseline files (these files already exist in the tarball).
- In Settings → Actions → General, keep the defaults (allow GitHub Actions).
- In Settings → Secrets and variables → Actions, add (optional but recommended): `SWH\\\_PERSONAL\\\_TOKEN` — Software Heritage API token to use Save Code Now with higher fairness.
- Ensure your default branch is `main`.
- Verify `metadata/releases.yaml` contains at least one release (see example in file).
> The `SourceCode` branch will be created by the \\\*\\\*Regenerate SourceCode\\\*\\\* workflow when you trigger it the first time.

### Contributor workflow (fully on GitHub)
- Fork the repo and create a branch.
- Upload raw files under `raw\\\_materials/<release-id>/...` (drag-and-drop works).
- Edit `metadata/releases.yaml` to add your release at the right position:
  - The order in the YAML list defines the commit order.
  - Each release can have multiple `sources` (archives and/or directories).
  - Use `strip\\\_components: auto|0|1|...` to flatten top-level dirs if needed.
- Open a Pull Request.

### What CI does on PR
- Validate & Dry-run build:
  - Runs `scripts/update\\\_metadata.py` to compute/append checksums and journal (in CI environment).
  - Runs `scripts/build\\\_source\\\_tree.py --clean` to generate `source\\\_code/<release-id>/...` for preview.
  - Uploads `source\\\_code/` as a PR artifact for inspection.

### Maintainer curation path
- Trigger Curate PR (maintainer) workflow (enter PR number). It:
  - Creates/updates a `curation/pr-<num>` branch in the base repo with built artifacts & metadata updates.
  - Opens/updates a PR from that branch into `main` for human review.
- After merging the curation PR to `main`:
  - Trigger Regenerate SourceCode branch to rewrite the synthetic history on `SourceCode` (force push).

### Archival
- Trigger Archive on Software Heritage to request a “Save Code Now” for this repo (or a custom URL input).

### Editing `metadata/releases.yaml` (manifest)
Example:
```yaml
releases:
  - id: "life\\\_090"
    title: "Wild\\\_Life 0.90"
    date: "1991-05-12"
    author: "Jane Doe <jane@example.org>"
    message: "Initial public release 0.90"
    sources:
      - path: "raw\\\_materials/life\\\_090/life\\\_090.tar.Z"
        strip\\\_components: "auto"

  - id: "life\\\_091"
    title: "Wild\\\_Life 0.91"
    date: "1991-08-23"
    author: "Jane Doe <jane@example.org>"
    message: "Maintenance update 0.91"
    sources:
      - path: "raw\\\_materials/life\\\_091/life\\\_091.zip"
        strip\\\_components: 1
```
Important: the array order here is the chronological order the commits will be created on `SourceCode` (first element = first commit).

### What the scripts do
- `extract\\\_any.py` — unpack archives or copy directories safely into a destination, with optional flattening (`strip\\\_components`).
- `build\\\_source\\\_tree.py` — read `metadata/releases.yaml` and build `source\\\_code/<id>/` for each release.
- `make\\\_synthetic\\\_history.py` — rewrite the `SourceCode` branch as an orphan branch with one commit per release (dates/authors/messages from manifest), then force-push.
- `update\\\_metadata.py` — compute SHA256 & size for all declared raw files (recursively for directories), append to `metadata/checksums.csv`, and append a line to
- `metadata/journal.md`.

### Supported input formats
- Via libarchive (`bsdtar`), p7zip, and ncompress the CI supports:
  - `.zip`, `.7z`
  - `.tar`, `.tar.gz` (`.tgz`), `.tar.bz2` (`.tbz`, `.tbz2`), `.tar.xz` (`.txz`)
  - legacy `.tar.Z` and rare `.Z` (if it’s an archive)
  - plain directories

### Reproducibility & safety
- Unpacking happens in a temp dir and is then moved into the build tree.
- Empty directories are preserved via `.emptydir` markers.
- Synthetic history author/commit dates are taken from the manifest; default commit time is noon UTC if only a date is provided.

### First run checklist
[ ] Add at least one release entry to `metadata/releases.yaml`.
[ ] Push a branch and open a PR to trigger Validate & Dry-run.
[ ] As a maintainer, run Curate PR, review & merge.
[ ] Run Regenerate SourceCode branch.
[ ] (Optional) Run Archive on Software Heritage.




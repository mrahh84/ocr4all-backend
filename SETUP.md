# ocr4all-backend – Setup Guide

## 1. Clone (already done)

```bash
git clone --recurse-submodules --remote-submodules https://github.com/mrahh84/ocr4all-backend.git
cd ocr4all-backend
```

## 2. Requirements

- **Java 17**
- **Maven** (`mvn`)
- **Docker** and **Docker Compose**
- **Bash** (optional, for `ocr4all-build.sh`)

### Install Java 17 and Maven (macOS with Homebrew)

```bash
brew install openjdk@17 maven
export PATH="/opt/homebrew/opt/openjdk@17/bin:$PATH"   # or /usr/local/opt/openjdk@17/bin on Intel
```

## 3. Environment

- A **`.env`** file is already present with repo-local paths (`./ocr4all-dev/`).
- For data under your home directory, copy and edit the example:
  ```bash
  cp docker-env-dev .env
  # Edit .env and replace ${HOME} with your actual home path, or use full paths.
  ```
- The directories `ocr4all-dev/assemble`, `ocr4all-dev/data`, `ocr4all-dev/tmp`, `ocr4all-dev/workspace/projects`, `ocr4all-dev/opt/ocr-d/resources`, and `ocr4all-dev/exchange` are created for Docker volumes.

## 4. Build

**Step 1 – Compile and package JARs (requires Java 17 and Maven):**

```bash
./ocr4all-build.sh build
```

**Step 2 – Build Docker images:**

```bash
docker compose build
```

## 5. Run

```bash
docker compose up
```

- **API base**: **http://localhost:9090**
- **Root**: Opening **http://localhost:9090/** redirects to the API documentation.
- **Swagger UI**: **http://localhost:9090/api/doc/swagger-ui.html** (or **/api/doc/swagger-ui/index.html**)
- Default admin: `admin` / `ocr4all`

## 6. Notes

- **Apple Silicon (M1/M2/M3)**: The compose file sets `platform: linux/amd64` so the Calamari/OCR-D images (no native ARM) run under emulation. First `docker compose build` can take 15–30+ minutes and may hit transient apt/network errors; re-run `docker compose build` if it fails.
- **Server image**: The main API image uses Eclipse Temurin (`eclipse-temurin:17-jre-jammy`) because the legacy `openjdk` image is no longer on Docker Hub.

## 7. Optional – OCR-D models

To use ocr-d processors, install models under:

- `ocr4all-dev/opt/ocr-d/resources/ocrd-calamari-recognize` (Calamari)
- `ocr4all-dev/opt/ocr-d/resources/ocrd-tesserocr-recognize` (Tesserocr)

See the [ocr-d resource list](https://github.com/OCR-D/core/blob/master/ocrd/ocrd/resource_list.yml) and the main [README.md](README.md).

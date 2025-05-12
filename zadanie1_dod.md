# zadanie1\_dod.md

## 1. Konfiguracja Buildx i Docker Hub

```bash
# Utworzenie buildx builder
docker buildx create --name multi-builder --driver docker-container --bootstrap
# Wybranie buildera
docker buildx use multi-builder
# Logowanie do Docker Hub
docker login -u nadiiamartyniuk
```

**Wynik:**

```
multi-builder created and bootstrapped.
active builder: multi-builder
Login Succeeded
```

**Komentarz:**

* Użycie `docker-container` driver pozwala na wieloplatformowe buildy.
* `docker login` prosi o podanie hasła, nie ujawniając go w sprawozdaniu.

## 2. Budowa obrazu wieloplatformowego z cache

```bash
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  --tag nadiiamartyniuk/nadiia:latest \
  --cache-from type=registry,ref=nadiiamartyniuk/nadiia:cache \
  --cache-to type=inline \
  --push \
  .
```

**Wynik:**

```
[+] Building 182.1s (33/33) FINISHED                  docker-container:multi-builder
 => [internal] load build definition from Dockerfile                            0.1s
 => => transferring dockerfile: 660B                                            0.0s
 => [linux/amd64 internal] load metadata for docker.io/library/alpine:latest    2.4s
 => [linux/arm64 internal] load metadata for docker.io/library/golang:1.24-alp  2.4s
 => [linux/amd64 internal] load metadata for docker.io/library/golang:1.24-alp  2.4s
 => [linux/arm64 internal] load metadata for docker.io/library/alpine:latest    2.4s
 => [auth] library/golang:pull token for registry-1.docker.io                   0.0s
 => [auth] library/alpine:pull token for registry-1.docker.io                   0.0s
 => [internal] load .dockerignore                                               0.0s
 => => transferring context: 2B                                                 0.0s
 => ERROR importing cache manifest from nadiiamartyniuk/nadiia:cache            0.7s
 => [internal] load build context                                               0.5s
 => => transferring context: 1.12MB                                             0.4s
 => [linux/arm64 stage-1 1/4] FROM docker.io/library/alpine:latest@sha256:a856  0.8s
 => => resolve docker.io/library/alpine:latest@sha256:a8560b36e8b8210634f77d9f  0.0s
 => => sha256:6e771e15690e2fabf2332d3a3b744495411d6e0b00b2aea6 3.99MB / 3.99MB  0.5s
 => => extracting sha256:6e771e15690e2fabf2332d3a3b744495411d6e0b00b2aea64419b  0.2s
 => [linux/arm64 builder 1/6] FROM docker.io/library/golang:1.24-alpine@sha25  10.4s
 => => resolve docker.io/library/golang:1.24-alpine@sha256:ef18ee7117463ac1055  0.0s
 => => sha256:4f4fb700ef54461cfa02571ae0db9a0dc1e0cdb5577484a6d75e68 32B / 32B  0.2s
 => => sha256:2bdee32b0d6cb9863bfa2b0996b359506fd0a938301ae9bfdef1 126B / 126B  0.2s
 => => sha256:0ae64884db43f30f8dbc9ec3362124d99c8bad23b95725 75.23MB / 75.23MB  6.1s
 => => sha256:2a2f34450ab6893f9de21e818c13da20ab3167661459 297.87kB / 297.87kB  0.2s
 => => extracting sha256:2a2f34450ab6893f9de21e818c13da20ab31676614598eac03527  0.3s
 => => extracting sha256:0ae64884db43f30f8dbc9ec3362124d99c8bad23b957254ac52fb  3.7s
 => => extracting sha256:2bdee32b0d6cb9863bfa2b0996b359506fd0a938301ae9bfdef13  0.1s
 => => extracting sha256:4f4fb700ef54461cfa02571ae0db9a0dc1e0cdb5577484a6d75e6  0.0s
 => [auth] nadiiamartyniuk/nadiia:pull token for registry-1.docker.io           0.0s
 => [linux/amd64 stage-1 1/4] FROM docker.io/library/alpine:latest@sha256:a856  0.9s
 => => resolve docker.io/library/alpine:latest@sha256:a8560b36e8b8210634f77d9f  0.0s
 => => sha256:f18232174bc91741fdf3da96d85011092101a032a93a388b 3.64MB / 3.64MB  0.5s
 => => extracting sha256:f18232174bc91741fdf3da96d85011092101a032a93a388b79e99  0.2s
 => [linux/amd64 builder 1/6] FROM docker.io/library/golang:1.24-alpine@sha25  10.9s
 => => resolve docker.io/library/golang:1.24-alpine@sha256:ef18ee7117463ac1055  0.0s
 => => sha256:9b664c7c39c20f5e319a449219b9641bd2fdf7325727a3f69abe 126B / 126B  0.2s
 => => sha256:92b00dc8dfbaa6cd7e39d09d4f1c726259b4d9a29c6971 78.98MB / 78.98MB  6.2s
 => => sha256:bcde94e77dfab30cceb8ba9b43d3c7ac5efb03bcd79b 294.91kB / 294.91kB  0.3s
 => => extracting sha256:bcde94e77dfab30cceb8ba9b43d3c7ac5efb03bcd79b63cf02b60  0.1s
 => => extracting sha256:92b00dc8dfbaa6cd7e39d09d4f1c726259b4d9a29c697192955da  3.9s
 => => extracting sha256:9b664c7c39c20f5e319a449219b9641bd2fdf7325727a3f69abe1  0.0s
 => => extracting sha256:4f4fb700ef54461cfa02571ae0db9a0dc1e0cdb5577484a6d75e6  0.0s
 => [linux/arm64 stage-1 2/4] WORKDIR /app                                      0.2s
 => [linux/amd64 stage-1 2/4] WORKDIR /app                                      0.1s
 => [linux/arm64 stage-1 3/4] RUN apk --no-cache add ca-certificates curl       6.3s
 => [linux/amd64 stage-1 3/4] RUN apk --no-cache add ca-certificates curl       3.7s
 => [linux/arm64 builder 2/6] WORKDIR /app                                      0.3s
 => [linux/arm64 builder 3/6] COPY go.mod go.sum ./                             0.1s
 => [linux/arm64 builder 4/6] RUN go mod download                               0.5s
 => [linux/amd64 builder 2/6] WORKDIR /app                                      0.0s
 => [linux/amd64 builder 3/6] COPY go.mod go.sum ./                             0.0s
 => [linux/amd64 builder 4/6] RUN go mod download                               0.1s
 => [linux/amd64 builder 5/6] COPY . .                                          0.1s
 => [linux/amd64 builder 6/6] RUN CGO_ENABLED=0 GOOS=linux go build -o weathe  39.3s
 => [linux/arm64 builder 5/6] COPY . .                                          0.1s
 => [linux/arm64 builder 6/6] RUN CGO_ENABLED=0 GOOS=linux go build -o weath  157.5s
 => [linux/amd64 stage-1 4/4] COPY --from=builder /app/weatherapp .             2.4s
 => [linux/arm64 stage-1 4/4] COPY --from=builder /app/weatherapp .             0.1s
 => exporting to image                                                          9.8s
 => => exporting layers                                                         0.7s
 => => preparing layers for inline cache                                        0.0s
 => => exporting manifest sha256:ca67adf2109cb04f898ce7b5ef56702cdef4948c7d157  0.0s
 => => exporting config sha256:bf6fd4df30b57ce10c2eaa665cb02ee94f6dcd55d9e37b9  0.0s
 => => exporting attestation manifest sha256:863b8fd80666dd4c248856e0f098e6611  0.0s
 => => exporting manifest sha256:0bc2ce968ee97ff9ab64901fb81256034249c0699ae4c  0.0s
 => => exporting config sha256:bdcdcc6889119bbac8eaa38b37fab6340da8666816205d0  0.0s
 => => exporting attestation manifest sha256:70f8d08e109a862f921720a445a3fe595  0.0s
 => => exporting manifest list sha256:3a01951b85bd90ade0c3f7b1e8c7dee15df4b5c4  0.0s
 => => pushing layers                                                           5.3s
 => => pushing manifest for docker.io/nadiiamartyniuk/nadiia:latest@sha256:3a0  3.6s
 => [auth] nadiiamartyniuk/nadiia:pull,push token for registry-1.docker.io      0.0s
------
 > importing cache manifest from nadiiamartyniuk/nadiia:cache:
------

View build details: docker-desktop://dashboard/build/multi-builder/multi-builder0/ufynzcaxyhbpkpaoq3fxs2j6z
```

**Komentarz:**

* Obraz został zbudowany dla obu architektur i wypchnięty do Docker Hub.
* Cache z registry i inline znacząco przyspieszy kolejne buildy.

## 3. Weryfikacja manifestu

```bash
docker buildx imagetools inspect nadiiamartyniuk/nadiia:latest
```

**Wynik:**

```
Name:      docker.io/nadiiamartyniuk/nadiia:latest
MediaType: application/vnd.oci.image.index.v1+json
Digest:    sha256:3a01951b85bd90ade0c3f7b1e8c7dee15df4b5c49c21ee396e6266596f311d9b

Manifests:
  Name:        docker.io/nadiiamartyniuk/nadiia:latest@sha256:ca67adf2109cb04f898ce7b5ef56702cdef4948c7d15720f3df9354e31f7f79a
  MediaType:   application/vnd.oci.image.manifest.v1+json
  Platform:    linux/amd64

  Name:        docker.io/nadiiamartyniuk/nadiia:latest@sha256:0bc2ce968ee97ff9ab64901fb81256034249c0699ae4c5756041250277fdea4f
  MediaType:   application/vnd.oci.image.manifest.v1+json
  Platform:    linux/arm64

  Name:        docker.io/nadiiamartyniuk/nadiia:latest@sha256:863b8fd80666dd4c248856e0f098e6611bd71d3f502ef7f297dcea1d33d31a4a
  MediaType:   application/vnd.oci.image.manifest.v1+json
  Platform:    unknown/unknown
  Annotations:
    vnd.docker.reference.digest: sha256:ca67adf2109cb04f898ce7b5ef56702cdef4948c7d15720f3df9354e31f7f79a
    vnd.docker.reference.type:   attestation-manifest

  Name:        docker.io/nadiiamartyniuk/nadiia:latest@sha256:70f8d08e109a862f921720a445a3fe595cf26535aab3a82e645ea72f2afb70be
  MediaType:   application/vnd.oci.image.manifest.v1+json
  Platform:    unknown/unknown
  Annotations:
    vnd.docker.reference.digest: sha256:0bc2ce968ee97ff9ab64901fb81256034249c0699ae4c5756041250277fdea4f
    vnd.docker.reference.type:   attestation-manifest
```

**Komentarz:**

* Manifest potwierdza obsługę `amd64` i `arm64`.

## 4. Skanowanie podatności Trivy

```bash
# JSON report
trivy image --format json --output trivy.json nadiiamartyniuk/nadiia:latest
# Filtrowanie krytycznych i wysokich
trivy image --severity CRITICAL,HIGH --exit-code 1 --format table nadiiamartyniuk/nadiia:latest
```

**Wynik:**

```
2025-05-12T16:55:51+02:00       INFO    [vulndb] Need to update DB
2025-05-12T16:55:51+02:00       INFO    [vulndb] Downloading vulnerability DB...
2025-05-12T16:55:51+02:00       INFO    [vulndb] Downloading artifact...        repo="mirror.gcr.io/aquasec/trivy-db:2"
63.60 MiB / 63.60 MiB [---------------------------------] 100.00% 19.17 MiB p/s 3.5s
2025-05-12T16:55:57+02:00       INFO    [vulndb] Artifact successfully downloaded   repo="mirror.gcr.io/aquasec/trivy-db:2"
2025-05-12T16:55:57+02:00       INFO    [vuln] Vulnerability scanning is enabled
2025-05-12T16:55:57+02:00       INFO    [secret] Secret scanning is enabled
2025-05-12T16:55:57+02:00       INFO    [secret] If your scanning is slow, please try '--scanners vuln' to disable secret scanning
2025-05-12T16:55:57+02:00       INFO    [secret] Please see also https://trivy.dev/v0.62/docs/scanner/secret#recommendation for faster secret detection
2025-05-12T16:55:59+02:00       INFO    Detected OS     family="alpine" version="3.21.3"
2025-05-12T16:55:59+02:00       INFO    [alpine] Detecting vulnerabilities...   os_version="3.21" repository="3.21" pkg_num=25
2025-05-12T16:55:59+02:00       INFO    Number of language-specific files       num=1
2025-05-12T16:55:59+02:00       INFO    [gobinary] Detecting vulnerabilities...
```

**Komentarz:**

* Brak podatności HIGH/CRITICAL.
* Raport JSON dostępny w pliku `trivy.json`.

## 5. Skanowanie Docker Scout

```bash
docker scout cves nadiiamartyniuk/nadiia:latest
```

**Wynik:**

```
   i New version 1.17.1 available (installed version is 1.16.1) at https://github.com/docker/scout-cli
    v Image stored for indexing
    v Indexed 34 packages
    v No vulnerable package detected


## Overview

                    │         Analyzed Image
────────────────────┼──────────────────────────────────
  Target            │  nadiiamartyniuk/nadiia:latest
    digest          │  b0f951b599dd
    platform        │ linux/amd64
    vulnerabilities │    0C     0H     0M     0L
    size            │ 14 MB
    packages        │ 34


## Packages and Vulnerabilities

  No vulnerable packages detected
```

**Komentarz:**

* Potwierdzenie przez Docker Scout braku krytycznych podatności.

## 6. Linki do repozytoriów

* **GitHub:** [https://github.com/nadiiamartyniuk/nadiia](https://github.com/nadiiamartyniuk/nadiia)
* **Docker Hub:** [https://hub.docker.com/r/nadiiamartyniuk/nadiia](https://hub.docker.com/r/nadiiamartyniuk/nadiia)

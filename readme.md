# Rust Dockerfile Collection

This repository is designed to simplify the process of building Docker images for Rust projects. Its primary purpose is to compare the performance and memory footprint among three Rust build targets: `x86_64-unknown-linux-gnu`, `x86_64-unknown-linux-musl`, and `x86_64-alpine-linux-musl`.

## Comparison of Rust Build Targets
Every image is built from the same source application (Actix Web 4 + Prometheus Metrics Endpoint) on the same machine (AKS single node B4s 16GB RAM)

---
### Dockerfile.glibc `x86_64-unknown-linux-gnu`

**Pros:**
- Fast response time (~0.3ms on the `/metrics` endpoint).
- Fast compile time (~6 minutes compiling dependencies + 6s compiling main app).
![image](https://github.com/bangbaew/rust-dockerfile-collection-cargo-chef/assets/29559559/d1785d68-dd28-45f3-ba6b-8e2a65f3ea4b)

**Cons:**
- High runtime memory usage (startup: 6MB, peak: 25MB).
- Not able to use `scratch` as final image
![image](https://github.com/bangbaew/rust-dockerfile-collection-cargo-chef/assets/29559559/583205ad-0a1f-4016-8c53-992328832054)

---
### Dockerfile.alpine `x86_64-alpine-linux-musl`

**Pros:**
- Low runtime memory usage (startup: 3MB, peak: 8MB).
- Able to use `scratch` as final image

**Cons:**
- Slow response time (~3ms on the `/metrics` endpoint).
- Slow compile time (~10 minutes compiling dependencies + 1 minute compiling main app).

---
### Dockerfile.musl `x86_64-unknown-linux-musl`

**Pros:**
- Low runtime memory usage (startup: 3MB, peak: 6MB).
- Fast compile time (~6 minutes compiling dependencies + 30s compiling main app).
- Able to use `scratch` as final image

![image](https://github.com/bangbaew/rust-dockerfile-collection-cargo-chef/assets/29559559/3bdf9598-5380-437d-bc7d-95a79dd9b6ca)

**Cons:**
- Medium response time (~0.7ms on the `/metrics` endpoint).

![image](https://github.com/bangbaew/rust-dockerfile-collection-cargo-chef/assets/29559559/9b61f7ca-de5c-453c-a428-5dfff6278124)

---
This comparison provides valuable insights into the trade-offs between different Rust build targets when building Docker images for Rust projects.
I haven't done any load-testing to compare runtime performance yet.
Feel free to explore and use these Dockerfiles to better understand and optimize your Rust applications in terms of performance and memory footprint.

# AFTERGLOW Studio

Two ComfyUI production workflows for Arabic-language vertical film/series and motion/product video. Source package: `AFTERGLOW_Studio_Two_Workflow.zip`.

## Current status

- The source ZIP and a manual image-build workflow are in this repository.
- The Docker image has **not** been built or published yet.
- Building requires Docker Build Cloud. Its free Docker Personal access is a **7-day trial**, after which paid access may be required. The first remote image build can consume trial time and may be billed after the trial. Review the current Docker Build Cloud offer before starting.
- No GPU pod is created by the GitHub workflow.
- This repository is public. Do not commit credentials, uploaded source PDFs/images, generated video, or Docker access tokens.

## One-time Docker Build Cloud setup

1. Sign in to Docker Hub as `tylersabrin`.
2. In Docker Build Cloud, create a builder. The workflow expects its name as a GitHub Actions variable.
3. In this repository, open **Settings → Secrets and variables → Actions**.
4. Add repository variable `CLOUD_BUILDER_NAME` with the builder name you chose (often `default`).
5. Add repository secret `DOCKER_ACCESS_TOKEN` containing a Docker Hub personal access token with read/write access. Create the token in Docker Hub's account security settings. Paste it into the GitHub Secret field; never commit or send it in a chat.

## Build and publish manually

Open **Actions → Build AFTERGLOW image (manual) → Run workflow**. The workflow does not run on pushes. Its default `publish=false` validates and builds to the remote build cache without publishing an image. To publish the versioned image `tylersabrin/afterglow-studio:1.0.0`, select `publish=true` and run the workflow. A complete image build can take substantial time; verify Docker Build Cloud pricing/trial status before starting.

After a successful publish, the image can be referenced by a RunPod template. The RunPod template itself is not created automatically.

## Running on RunPod without persistent storage

The image preinstalls ComfyUI and software; model weights remain out of the image. At pod startup, the selected profile downloads its models automatically. The full native H3/Qwen weights alone are about 156 GB, and supporting planning/speech models add more. Fresh pods without persistent storage must download the weights again. Inputs, projects, continuity records, and outputs on ephemeral pod storage are lost when the pod is terminated; export needed work first.

The boot disk setting in `runpod/create_template.py` is an initial 500 GB estimate, not a measured minimum or guarantee. Check free disk and GPU memory for your chosen profile before launching. Production model inference has not been tested on a RunPod GPU.

## Model profiles

Set the template environment variable `AFTERGLOW_PROFILE` to `film`, `design`, `graphics`, or `full`. The graphics profile omits H3 video weights; choose `Graphic motion` in the design workflow. All profiles include Arabic speech. There is no music-generation model.

See `runpod/README.md` in the ZIP for build and runtime details.

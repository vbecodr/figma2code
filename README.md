# figma2code

This repository is a small set of GitHub Actions workflow examples for moving design tokens from Figma into a code publishing pipeline.

It is intended to be paired with the Figma plugin [HaKa](https://www.figma.com/community/plugin/1640753412567944113), but it can also be used independently. On its own, this repo is still useful as a minimal starting point for fetching Figma variables through the REST API and wiring them into your own token transformation and publishing pipeline. Using it with HaKa is simply more convenient because the payload handoff is already designed for that workflow, so you mostly just need to complete the setup and add your project-specific transform and publish steps.

## What this is for

Use this repo as a starting point when you want to:

- trigger a token publishing flow manually from GitHub Actions
- fetch Figma variables directly from the Figma REST API
- accept a prebuilt variables payload from another system such as HaKa
- transform that payload into your team-specific token format
- publish the generated output as files, packages, or build artifacts

This repo is intentionally minimal. It does not contain the token transformation logic yet. The workflows stop at placeholder steps where your team should add its own conversion and publishing commands, whether you run it as a standalone Figma-to-code pipeline or pair it with HaKa for a more ready-made handoff.

## Included workflow examples

### `.github/workflows/publish-tokens-from-figma-rest.example.yml`

This workflow shows how to:

- accept a `figma_file_key` and target `environment`
- read a `FIGMA_ACCESS_TOKEN` from GitHub Secrets
- download local variables from the Figma REST API
- save the response to `haka-variables.json`
- hand off to a future transform/publish step

Use this example when GitHub Actions should pull token data from Figma directly.

### `.github/workflows/publish-tokens-with-payload-json.example.yml`

This workflow shows how to:

- accept a token payload as `variables_payload_json`
- record the payload format with `variables_payload_format`
- write the incoming JSON to `haka-variables.json`
- hand off to a future transform/publish step

Use this example when another system already collected the variables and just needs GitHub Actions to continue the pipeline.

## Typical usage

1. Copy the workflow that matches your setup into a real workflow file in `.github/workflows/`.
2. Use the Figma REST example if this repo will run independently, or use the payload JSON example if HaKa or another system will provide the variables payload.
3. Replace the `TODO` steps with your token transformation logic.
4. Add your publishing step, such as committing generated files, publishing a package, or uploading artifacts.
5. If you use the Figma REST example, create a repository secret named `FIGMA_ACCESS_TOKEN`.

## Expected customization

You will likely want to add:

- schema validation for the incoming JSON
- mapping from Figma variables to your code token structure
- environment-specific output paths
- CI checks or pull request automation
- publishing to npm, a monorepo package, or a design token artifact store

## In short

This repo is not a complete token toolchain. It is a lightweight template repository for teams that need a clear starting point for turning Figma variables into code artifacts through GitHub Actions, either independently through the Figma REST API or with a more convenient prewired payload handoff from HaKa.

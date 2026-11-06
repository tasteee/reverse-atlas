# reverse-atlas

## Product Specification

### 1. Overview

RE Archive is a community-driven repository and discovery platform for reusable reverse-engineering analyses of software.

The platform allows researchers to publish, discover, download, and reuse existing software analyses rather than repeatedly performing the same initial reverse-engineering work.

It is designed primarily around Ghidra and REA (Reverse Engineer Anything), while remaining extensible to other reverse-engineering tools and formats.

**Core principle:** Reverse-engineering work should be reusable. An analysis performed by one person should be useful to everyone else investigating the same software.

### 2. Goals

- Eliminate duplicated reverse-engineering effort.
- Create a searchable directory of analyzed software.
- Allow researchers to share complete Ghidra analyses and REA snapshots.
- Make analyses easy for AI coding agents to discover and consume.
- Establish reliable binary identity and version matching.
- Build a community knowledge base around software internals.
- Encourage contributions, corrections, and improvements to existing analyses.

### 3. Core Concepts

#### Software
A product being analyzed, such as Scaler 2, Photoshop, or a particular game.

A software entry can contain multiple versions, architectures, binaries, and analyses.

#### Binary
An individual executable, DLL, shared library, or other compiled software artifact.

Each binary should have its own identity, primarily determined by its cryptographic hashes.

#### Analysis
A reverse-engineering result associated with a specific binary.

An analysis may contain:
- Ghidra project or exported analysis data.
- REA snapshot.
- Function information.
- Defined strings.
- Symbols and labels.
- Cross-references.
- Data types and structures.
- Analysis configuration.
- Research notes and annotations.

One binary can have multiple analyses from different contributors or using different tools and configurations.

#### Contribution
An uploaded analysis, update, annotation, correction, or improvement submitted by a community member.

### 4. Website Structure

The main navigation should contain:

- **Explore:** Browse and search the software archive.
- **Categories:** Discover software by category.
- **Contribute:** Upload a new analysis or improve an existing one.
- **About:** Explain the project, supported formats, contribution guidelines, and integration capabilities.

User account functionality should support contributor profiles, uploads, and contribution history.

### 5. Software Directory

The directory is the primary discovery interface.

Users should be able to:

- Search by software name.
- Search by vendor or developer.
- Search by binary filename.
- Search by SHA-256 hash.
- Filter by software category.
- Filter by architecture.
- Filter by available analysis formats.
- Filter by software version.
- Sort by recently analyzed, recently updated, or most downloaded.

Suggested categories:
- Audio / Music Software
- Video / Image Software
- Development Tools
- Games
- Productivity Software
- Operating Systems
- Security Software
- Utilities
- Other

Each software listing should display:
- Product name and icon or logo.
- Developer/vendor.
- Category.
- Latest analyzed version.
- Number of available binaries.
- Number of available analyses.
- Supported analysis formats.
- Last updated date.

### 6. Software Detail Page

Each software product should have a dedicated page.

The page should contain:

**Overview**
- Software name.
- Developer.
- Description.
- Website.
- Category.
- Available versions.

**Binaries**

A list of analyzed binaries belonging to the product.

For each binary:
- Filename.
- File format.
- Architecture.
- File size.
- SHA-256.
- Version/build association, if known.
- Image base, if applicable.
- Section information.

**Available Analyses**

Each analysis should display:
- Analysis format.
- Tool and version.
- Contributor.
- Date created.
- Last updated.
- Number of functions discovered.
- Number of defined strings.
- Analysis coverage or completion status, when known.
- Download size.
- Description and notes.

Users should be able to inspect an analysis before downloading it.

### 7. Analysis Downloads

The platform should support multiple analysis formats.

Initial priorities:

1. REA snapshots.
2. Ghidra projects or appropriately packaged Ghidra analysis exports.

Future possibilities:
- IDA databases.
- Binary Ninja databases.
- radare2/rizin analysis exports.
- Other compatible reverse-engineering formats.

Every download must include a manifest describing its contents and compatibility requirements.

The platform must never assume that an analysis is compatible with a binary simply because the software product name matches.

**Binary verification is mandatory.**

Before using a downloaded analysis, REA should be able to compare its expected binary hash against the user's local binary.

If the hashes do not match, the user should receive a clear warning rather than silently loading potentially incompatible analysis data.

### 8. Upload and Contribution System

Users should be able to submit analyses through a guided upload process.

Suggested workflow:

1. Search for the software.
2. Select an existing software entry or create a new one.
3. Select or create the associated binary entry.
4. Upload an analysis file.
5. Provide analysis metadata.
6. Submit for processing.
7. Receive a confirmation when the analysis is available.

Required metadata:
- Software name.
- Binary filename.
- SHA-256 hash.
- Analysis format.
- Tool name and version.
- Architecture.
- Contributor.

Optional metadata:
- Software version.
- Operating system.
- Analysis settings.
- Description.
- Research notes.
- Known limitations.
- Specific features investigated.

The system should calculate and verify uploaded file hashes where appropriate.

### 9. Analysis Processing and Validation

Uploads should be processed asynchronously.

The processing pipeline should:

- Validate the archive format.
- Scan for malicious files.
- Calculate file hashes.
- Extract manifest information.
- Check for duplicate uploads.
- Verify declared metadata against available contents.
- Generate download metadata.
- Mark the upload as ready, rejected, or requiring review.

Large files should be supported through resumable uploads where practical.

The platform should distinguish between:
- Verified metadata.
- Contributor-provided metadata.
- Automatically extracted metadata.

### 10. REA Integration and API

REA integration is one of the platform's most important features.

The archive should expose a documented API that allows REA and other clients to:

- Search for software.
- Search by binary hash.
- Retrieve binary metadata.
- List available analyses.
- Check compatibility.
- Download compatible analysis packages.
- Retrieve manifests and contribution information.

The ideal REA workflow:

1. REA identifies the binary being investigated.
2. REA calculates its SHA-256 hash.
3. REA queries RE Archive.
4. The archive returns matching binary records and compatible analyses.
5. The user or AI agent selects an analysis.
6. REA downloads and loads the analysis.
7. The agent continues investigating without repeating the initial analysis process.

The API should be usable independently of the website, allowing AI agents and other reverse-engineering tools to interact with the archive directly.

### 11. Community Features

Initial community functionality should remain relatively lightweight.

- Contributor profiles.
- Upload history.
- Download counts.
- Analysis descriptions.
- Comments or annotations.
- Report incorrect or problematic uploads.
- Submit improved versions of existing analyses.

Contributors should receive attribution when their work is downloaded or reused.

The system should preserve analysis history rather than silently replacing previous contributions.

### 12. Trust, Safety, and Legal Considerations

The archive should distribute analysis artifacts, not proprietary software binaries.

Important safeguards:
- Uploaders must confirm they have the right to distribute submitted analysis artifacts.
- Clear acceptable-use and contribution policies.
- Malware scanning and archive validation.
- No execution of uploaded binaries on the production infrastructure.
- Rate limiting and abuse prevention.
- A process for reporting and removing infringing or harmful material.
- Clear warnings about potentially sensitive information contained in analysis artifacts.

The platform should not imply that Ghidra-generated output is automatically free of intellectual property restrictions.

### 13. Technical Direction

The implementation should prioritize reliability, efficient large-file handling, and a clean separation between the website and its API.

Suggested architecture:

- Frontend: Modern TypeScript-based web application.
- Backend: API-driven service.
- Database: PostgreSQL.
- Object storage: S3-compatible storage for analysis packages.
- Search: PostgreSQL full-text search initially, with room for dedicated search infrastructure later.
- Background processing: Asynchronous upload validation and metadata extraction.
- Authentication: Account-based authentication for contributors.
- API: Versioned REST API with machine-readable manifests.

Analysis files should be stored separately from relational metadata.

The architecture should accommodate very large analysis packages without requiring them to pass through ordinary application-server memory.

### 14. Initial MVP

The first release should be deliberately focused.

**Must have:**
- Searchable software directory.
- Software detail pages.
- Binary records and SHA-256 fingerprints.
- Analysis listings.
- REA snapshot uploads and downloads.
- Ghidra analysis package uploads and downloads.
- Contributor attribution.
- Basic upload validation.
- Binary compatibility verification.
- Public API for searching and downloading analyses.

**Can wait:**
- Comments and discussions.
- Contributor reputation.
- Advanced search infrastructure.
- Automated Ghidra analysis.
- Support for additional reverse-engineering tools.
- AI-generated analysis summaries.
- Collaborative research workspaces.
- Advanced version comparison.

### 15. Long-Term Vision

RE Archive should eventually become a shared infrastructure layer for reverse engineering.

Imagine an AI coding agent investigating a particular feature in an application. Instead of beginning with an unexplored binary, it can query the archive, retrieve an existing analysis, inspect available research, and immediately begin investigating the relevant implementation.

As more researchers contribute, the archive becomes increasingly valuable.

The long-term objective is to make reverse-engineering knowledge cumulative rather than isolated: every new analysis should make future investigations easier.

---
name: maintain-recipkg-standard-library
description: "Curate the Recipkg software recipe catalog: identify category gaps, assess high-star open-source candidates, publish or update standalone recipes, and keep coverage evidence and the visual index accurate. Use when adding, upgrading, or organizing standard-library entries."
---

# Maintain the Recipkg standard library

Use this skill when the task is to add, upgrade, or organize a recipe in the Recipkg standard library. For extracting one user-selected software project without catalog work, use `software-to-prompt`; for dependency analysis or integration, use `discover-dependencies` and `add-dependency`.

## Select a useful candidate

1. Read the standard-library README and its `AGENTS.md`; inspect existing recipes in the relevant category before proposing another entry. Prefer closing a real category or capability gap over increasing the item count.
2. Verify the upstream repository, release tag, full commit, license files, latest activity, and GitHub star count from authoritative sources. Record the star count and observation date in `SOURCES.md` when selection is based on popularity. Stars are a discovery signal, not evidence of quality, completeness, or compatibility. Do not describe a project as open source when its license is unclear or source-available only.
3. Compare reasonable candidates within the category. Record why the selected source fits and which parts of the software are in scope. Fix the source to a reviewed release commit; do not use a moving branch as the recipe version.
4. Check repository size before downloading or cloning. If the action may create or transfer more than 100 MB, use the available `space-first` workflow first. Prefer official raw files, API metadata, sparse checkout, or a narrowly scoped clone when those provide enough evidence. State the sampling boundary.

## Build a reviewable recipe

- Use `software-to-prompt` to extract behavior from the fixed source. Record observed behavior, inferred behavior, license, dependencies, acceptance conditions, and unknowns separately.
- Consult the standard library with `discover-dependencies` and `add-dependency`. Read candidate `RECIPE.md`, `SOURCES.md`, `COVERAGE.md`, and recursive dependencies. Add only dependencies that serve the target; design and quality recipes are guidance, not runtime services.
- Keep the canonical recipe in its own public Git repository with the `recipkg` topic. Recipe payloads contain UTF-8 Markdown only. Keep implementation source, binaries, scripts, screenshots, databases, secrets, and installation hooks outside the archive.
- Include enough material to make the target and remaining gaps reviewable: `RECIPE.md`, `BUILD_PROMPT.md`, `ACCEPTANCE.md`, `SOURCES.md`, `COVERAGE.md`, and a clear `README.md`. Add a scoped `AGENTS.md` when it carries package-specific maintenance rules.
- Make the README easy to scan. Show the product boundary and dependencies with a diagram when that clarifies the system; retain the standard-library cover and consistent links when editing the index.

## Validate and publish

1. Explicitly run `recipkg pack`; then run `recipkg check` on the resulting archive. Source-directory checks and prompt rendering remain offline and must not silently fetch Git dependencies.
2. Build the archive prompt in both `zh-CN` and `en`, run `recipkg discover`, and inspect the archive for Markdown-only payloads, complete recursive dependency context, exact source pins, and valid lock metadata.
3. When testing version control or deterministic packaging, pack the same input twice and compare archive bytes. Do not refresh a fixed dependency pin merely because the containing repository has a newer commit; compare the dependency subdirectory and recursive content first. Update the package version and lock when the included dependency content changes.
4. Publish the recipe repository and verify its public visibility, `recipkg` topic, branch commit, and raw Markdown. Only then add or update its row in the standard-library README with category, fixed upstream version, and a direct coverage link. Commit and verify the index separately.
5. Keep archive checks, prompt builds, upstream tests, independent reconstruction, and live product acceptance as separate evidence. Mark the recipe complete only after the stated scope has actually been reconstructed and its acceptance has passed.

If source, license, dependency, or runtime evidence is missing, leave the recipe in progress and name the exact gap. Do not fill a category with an unrelated package or overstate what a static source review proves.

# Small fixes, visible consequences

Four merged contributions, with the problem, the change, and the evidence.
These notes describe the merged patches; linked pull requests record the original
validation and its limits.

## A click that kept dragging

In Bento Slides, a quick click could release the mouse while the editor was still
waiting to change its selection target. Replaying the original press after that
wait started a drag whose release had already happened. The object followed the
cursor even though the user had finished clicking.

The patch watches for mouse-up before the wait and cancels that stale handoff.
A held-button drag keeps the existing path. The pull request records real-browser
checks of both fast clicks and deliberate drags, plus TypeScript and build checks.

[Bento #366](https://github.com/nyblnet/bento/pull/366) ·
[Merged code](https://github.com/nyblnet/bento/commit/4a6e250200d8c4a4dfe63060b1903cf3c8df7fb3) · September 2, 2026

## Two vertices became one

In three.js, `mergeVertices()` converted quantized coordinates to signed 32-bit
integers while building its hashes. At zero or very small tolerance, that conversion
could wrap distinct coordinates to the same value.

The merged patch uses `Math.trunc()` and also preserves wider quantized coordinates
in `toCreasedNormals()`. Its regression feeds `mergeVertices()` positions at x=11,
x=12, and x=11 with zero tolerance. The expected result has two positions and
indices `[0, 1, 0]`. The original submission records a focused runtime check;
it does not claim a locally completed full QUnit run.

[three.js #34390](https://github.com/mrdoob/three.js/pull/34390) ·
[Merged code and regression](https://github.com/mrdoob/three.js/commit/af4ff3e02f9b7ceaefd8e482fbbf3f8ee36b1203) · August 29, 2026

## A path changed when it was read

Windows PowerShell 5.1 could read a BOM-less UTF-8 `feature.json` using the active
ANSI code page. A feature-directory name containing Chinese characters could be
corrupted before JSON parsing.

The Spec Kit patch makes both reads explicitly UTF-8. The regression writes a
non-ASCII directory name and checks the exact resolved path under Windows
PowerShell 5.1. That Windows-specific test was not run locally on macOS; the pull
request states the limitation.

[Spec Kit #4359](https://github.com/github/spec-kit/pull/4359) ·
[Merged code and regression](https://github.com/github/spec-kit/commit/59dc772b47b5d765ee8a920c3ccfd6dbac5bd1ec) · August 28, 2026

## Audio output missing from a trace

Braintrust's Java instrumentation handled OpenAI responses containing `choices`
or `output`. Audio transcription and translation responses use a `text` field,
so their output was missing from `braintrust.output_json`.

The patch retains the complete text-keyed response, including verbose segments
and word timestamps. Existing `choices` and `output` precedence is preserved.
The tests cover plain and verbose transcription responses; the pull request
records the focused test class and Gradle checks.

[Braintrust Java SDK #169](https://github.com/braintrustdata/braintrust-sdk-java/pull/169) ·
[Merged code and tests](https://github.com/braintrustdata/braintrust-sdk-java/commit/daaa975fbdebd92e5172ac6ec9e2234453e4e62d) · August 25, 2026

These personal contributions used AI assistance. The linked diffs, tests, and
maintainer merge decisions are the evidence for each result.

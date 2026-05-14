# Contributing

This document describes how to sign, translate, or propose changes to the Boundary-First Engineering manifesto.

## Signing

To add your name to [SIGNATORIES.md](./SIGNATORIES.md):

1. Fork this repository.
2. Open `SIGNATORIES.md` and add a line under "Founding Signatories" (or whichever section is appropriate when you sign) in this format:
   ```
   - **Your Name**, Role, Affiliation (optional)
   ```
   The affiliation is optional. If you do not want a public affiliation, leave it off. Example:
   ```
   - **Omer Morad**, Creator of Suites
   - **Jane Doe**, Staff Engineer
   ```
3. Open a pull request titled `Sign: Your Name`.
4. Once merged, your signature is permanent in the git history. You may withdraw your signature at any time by opening a PR that removes your line.

By signing, you endorse the principles of the manifesto as written. You do not gain or transfer copyright; the manifesto remains licensed under CC BY 4.0.

## Translating

Translations into any language are explicitly welcomed under CC BY 4.0.

1. Fork this repository.
2. Create a new file named `MANIFESTO.<lang>.md` using the two-letter ISO 639-1 language code. Example: `MANIFESTO.he.md` for Hebrew, `MANIFESTO.es.md` for Spanish.
3. Translate the full text of `MANIFESTO.md`, preserving structure and footnotes.
4. Add a line at the top of your translated file:
   ```
   *Translated from the canonical English version by [Your Name]. The canonical version is [MANIFESTO.md](./MANIFESTO.md).*
   ```
5. Open a pull request titled `Translation: <Language>`.

Translations are equal in status to the canonical version for readers of that language. The English version remains the canonical reference for textual disputes.

## Proposing Changes

The manifesto's prose is intentionally stable. Changes are not encouraged except to correct factual errors, fix typos, or update the GitHub-scaling footnote as new evidence accumulates.

For substantive disagreements with the principles, the recommended path is:

- **Open an issue** explaining your disagreement. Discussion is welcomed.
- **Write a response.** A blog post, a counter-manifesto, a fork: these are all legitimate ways to engage. The manifesto's value comes from being contestable.
- **Do not open PRs that change the value pairs or principles** without first discussing in an issue. The maintainers will close such PRs without review.

For corrections (typos, broken links, formatting), open a PR titled `Fix: <description>`.

## Adding to Lineage

If you believe a thinker or work belongs in [LINEAGE.md](./LINEAGE.md), open an issue first. Include the name, the work, and a one-sentence description of how their ideas connect to a specific principle in the manifesto. Maintainers will evaluate based on whether the connection is structural (the manifesto directly extends or cites the work) or merely thematic.

## Governance

The manifesto is hosted under the [suites-dev](https://github.com/suites-dev) organization but is not owned by Suites. It is collectively held by its signatories and licensed under CC BY 4.0.

Maintainers handle the technical operation of the repository (merging PRs, closing stale issues, keeping translations in sync). Maintainers do not have authority to change the principles or value pairs without broad signatory consensus.

If a substantive disagreement arises that cannot be resolved through discussion, the appropriate path is a fork. The Boundary-First Engineering manifesto does not claim to be the final word; it claims to be one position, defensibly held.

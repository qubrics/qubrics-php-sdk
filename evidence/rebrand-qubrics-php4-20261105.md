# PHP umbrella SDK Qubrics rename

Goal: ship `qubrics/sdk` at the next major, using `Qubrics\Sdk` and Qubrics 3.x dependencies, per #4 and [A11](https://github.com/qubrics/qubrics/blob/main/specs/rebrand-qubrics/spec.md).

Status: coded, not merged. Candidate `3.0.0` derives from latest remote SDK tag `v2.1.0`. No tags, publication or Packagist mutations performed.

## Correctness

- `composer.json` names `qubrics/sdk`, maps `Qubrics\Sdk\` to `src/`, and requires `qubrics/{core,sending,mailbox,management}` `^3.0`.
- `src/Sdk.php` exposes `Qubrics\Sdk\Sdk::VERSION = '3.0.0'`.
- README includes the package/import/exception migration, qubrics.com links, preferred environment names with legacy fallbacks and a registry-availability guard.
- Existing third-party requirements, credential prefixes, factories and wire contracts remain compatible. Historical changelog entries and licence attribution are unchanged.

## Red and green

Added `tests/public-contract.php`, exercising the real Composer consumer boundary across all five package identities and the three public client factories.

- Red before implementation: exit 255, `RuntimeException: Composer must install qubrics/core`.
- Green after the five package renames: exit 0, `Qubrics SDK public contract passed`.
- Checks: Composer strict validation, PHP syntax for source/test and `git diff --check`, all exit 0.
- Local dependencies use actual sibling worktrees through Composer path repositories with candidate version `3.0.0`. This proves local resolution and autoloading; it does not prove Packagist availability.
- CI checks out each dependency from main, resolves the candidate through an alternate temporary Composer manifest, then runs the public contract. Original shipped metadata is validated first.
- Local PHP 8.4.10, Composer 2.8.9; CI uses PHP 8.2.
- The baseline install's untracked old-name lockfile was moved into artifacts. No assertion or dependency constraint was weakened.

Logs under MAIN `.claude/artifacts/rebrand-qubrics-php4/`: `sdk-baseline-install.log`, `sdk-candidate-install.log`, `sdk-public-contract-red.log`, `sdk-public-contract-green.log`, `sdk-baseline-composer.lock`.

## Review and publication

Independent copy critique corrected the registry guard, dependency wording and explicit old/new exception migration. One humanize pass on changed prose and a protected-set check completed; technical identifiers, figures and URLs preserved.

Canonical publisher blocker: SDK monorepo main `2498ae8e90932fa98b54926653a4a6185bb2a4c3`, `scripts/publish-php-split.mjs:40-60`, enforces exact source/split parity and still requires `sendmux/sdk`. Source/generator/publisher alignment requires its own owner; the publication guard is not bypassed.

Remaining: settled PR review and merge, exact-version Ops tag approval, operator submissions of all five repositories, public `composer require` resolution, and abandonment replacements on all five old packages. Keep #4 open until measured evidence covers every acceptance box. No browser journey applies; no background resources created.

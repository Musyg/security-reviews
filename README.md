# Security Reviews

Reproducible smart-contract security reviews, one repository per vulnerability class. Each
review ships a deliberately vulnerable target, an exploit proof of concept, a `fixed` branch
that neutralises it, and a written report. Every PoC runs in CI on both branches, so the
results can be checked without trusting a screenshot.

## Reviews

| Vulnerability class | Review | Finding | Severity | Tests |
|---|---|---|---|---|
| ERC-4626 inflation | [erc4626-inflation-audit](https://github.com/Musyg/erc4626-inflation-audit) | First-deposit share inflation drains the next depositor | High | ![tests](https://github.com/Musyg/erc4626-inflation-audit/actions/workflows/ci.yml/badge.svg) |
| Signature malleability | [eip712-signature-replay-audit](https://github.com/Musyg/eip712-signature-replay-audit) | An ECDSA-malleable twin replays a signed claim for a double-spend | High | ![tests](https://github.com/Musyg/eip712-signature-replay-audit/actions/workflows/ci.yml/badge.svg) |
| Reward accounting drift | [reward-accounting-drift-audit](https://github.com/Musyg/reward-accounting-drift-audit) | A stale accumulator on deposit lets a late depositor steal accrued rewards | High | ![tests](https://github.com/Musyg/reward-accounting-drift-audit/actions/workflows/ci.yml/badge.svg) |
| Oracle, reentrancy, rounding | [stvault-audit](https://github.com/Musyg/stvault-audit) | Lending vault: stale-oracle drain, cross-function reentrancy, fee under-charge | High, Medium, Low | ![tests](https://github.com/Musyg/stvault-audit/actions/workflows/ci.yml/badge.svg) |
| Access control (Vyper) | [vyper-access-control-audit](https://github.com/Musyg/vyper-access-control-audit) | An unguarded ownership transfer lets any account seize control and drain the vault | High | ![tests](https://github.com/Musyg/vyper-access-control-audit/actions/workflows/ci.yml/badge.svg) |

## How each review is structured

Two branches:

- `master`: the vulnerable contract and a passing PoC that exploits it.
- `fixed`: the remediated contract and the same scenario, now neutralised.

Plus a written report (Markdown and PDF) stating impact, root cause, and remediation.

Run any of them:

```bash
git clone https://github.com/Musyg/<review>.git
cd <review>
forge install
forge test -vv      # master: the exploit
git checkout fixed
forge test -vv      # fixed: neutralised
```

## Note

These are demonstrations on intentionally vulnerable code, written to show methodology end
to end. They are not production code and must never be deployed. Each finding is a real
vulnerability in its demo contract, proven with an executable proof of concept.

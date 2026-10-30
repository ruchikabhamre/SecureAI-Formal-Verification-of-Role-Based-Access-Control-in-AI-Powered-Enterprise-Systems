# SecureAI: Formal Verification of Role-Based Access Control in AI-Powered Enterprise Systems

| | |
|---|---|
| **Team** | TAG |
| **Members** | Swapnil Jadhavar (swapniljadhavar7@gmail.com), Ruchika Bhamre(ruchikabhamare035@gmail.com) |
| **Verification tool** | [Dafny](https://dafny.org) (with the Z3 SMT solver) |
| **Repository** | [https://github.com/swapniljadhavar7/SecureAI-Formal-Verification-of-Role-Based-Access-Control-in-AI-Powered-Enterprise-Systems] |
| **Status** | Proposal stage: starter model and first lemmas included to show feasibility |

---
## 1. Problem statement

Enterprise AI platforms give people access to sensitive **documents**, **datasets** and **AI models**. Incorrect permission handling or role management can result in unauthorized access and security vulnerabilities.

## 2. Motivation

Traditional testing cannot exhaustively cover every combination of users, roles, permissions and state changes. We want to use **formal verification** to establish *machine-checked correctness guarantees* for a role-based authorization system, guarantees that hold for **all** states, not just the ones we thought to test.

## 3. Proposed work

1. Design and implement a role-based access control (RBAC) model for an AI platform.
2. Specify the authorization rules in Dafny.
3. Formally verify:
   - **P1 Unauthorized-access prevention** (default deny; access only via a role that grants it)
   - **P2 Role revocation correctness** (a revoked role no longer grants access, and revocation never escalates privileges)
   - **P3 Separation of duties** (no user ever holds two conflicting roles)
   - **P4 Invariant preservation** (every operation keeps the system in a valid state)
4. Demonstrate verification of **valid** implementations and of **deliberately faulty** ones (the faulty ones must be rejected by the verifier).
5. Evaluate the approach and document its limitations.

Details of each property are in [`docs/properties.md`](docs/properties.md).

## 4. Expected outcome

A small, formally verified RBAC prototype for an enterprise AI platform, including:

- machine-checked proofs (Dafny),
- an illustrative application (`src/Demo.dfy`),
- tests (`tests/RbacTests.dfy`),
- technical documentation (`docs/`).

## 5. Quick start

Install [Dafny 4.x](https://github.com/dafny-lang/dafny/releases), then:

```bash
./scripts/check.sh                 # verify, test, and confirm faulty code is rejected
dafny verify src/Rbac.dfy          # verify only the core model
dafny run src/Demo.dfy             # run the demo application
dafny test tests/RbacTests.dfy     # run runtime tests
```

## 6. Team

| Name | Contact |
|---|---|
| Swapnil Jadhavar | swapniljadhavar7@gmail.com |
| Ruchika Bhamre | [ruchikabhamare035@gmail.com] |

## License

MIT, see [`LICENSE`](LICENSE).

# SecureAI: Formal Verification of Role-Based Access Control in AI-Powered Enterprise Systems

> **Project proposal for the Software Verification course.**
> Dear Professor, we would like to propose the project described in this repository for the Software Verification course. We would appreciate your feedback on the topic and its suitability for the course.

| | |
|---|---|
| **Team** | TAG |
| **Members** | Swapnil Jadhavar (swapniljadhavar7@gmail.com), Ruchika Bhamre |
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

## 5. Repository layout

```
.
├── README.md                     <- this proposal
├── PROPOSAL.md                   <- proposal letter, scope, milestones, risks
├── docs/
│   ├── properties.md             <- properties to be verified, mapped to lemmas
│   ├── plan.md                   <- milestones and evaluation criteria
│   └── limitations.md            <- what the proofs do and do not guarantee
├── src/
│   ├── Rbac.dfy                  <- RBAC model, operations, invariants, lemmas
│   ├── Platform.dfy              <- example AI-platform policy (roles, perms, conflicts)
│   └── Demo.dfy                  <- runnable illustrative application
├── examples/faulty/
│   └── FaultyRbac.dfy            <- deliberately buggy versions (expected to FAIL)
├── tests/
│   └── RbacTests.dfy             <- runtime tests (dafny test)
├── scripts/check.sh              <- verify + test + check faulty code is rejected
└── .github/workflows/verify.yml  <- CI
```

## 6. Quick start

Install [Dafny 4.x](https://github.com/dafny-lang/dafny/releases), then:

```bash
./scripts/check.sh                 # verify, test, and confirm faulty code is rejected
dafny verify src/Rbac.dfy          # verify only the core model
dafny run src/Demo.dfy             # run the demo application
dafny test tests/RbacTests.dfy     # run runtime tests
```

## 7. Model at a glance

- **Resources**: documents, datasets and models, identified by name (e.g. `dataset:customers`, `model:churn`).
- **Permission**: an *action* (`Read`, `Write`, `Invoke`, `Train`, `Approve`) on a resource.
- **Roles** grant sets of permissions; **users** hold sets of roles.
- **Conflicts** are pairs of roles that no single user may hold together (separation of duties), e.g. `DataScientist` and `ModelApprover`: whoever trains a model must not approve it.
- **Access decision**: allow iff one of the user's roles grants the permission; deny otherwise.

## 8. Limitations (summary)

The proofs cover the *model and its Dafny implementation*, not authentication, networking, persistence, concurrency or a deployed AI system. See [`docs/limitations.md`](docs/limitations.md).

## 9. Team

| Name | Contact |
|---|---|
| Swapnil Jadhavar | swapniljadhavar7@gmail.com |
| Ruchika Bhamre | [email] |

## License

MIT, see [`LICENSE`](LICENSE).

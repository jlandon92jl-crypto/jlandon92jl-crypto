### Joey Landon

I build trading systems and tooling for coding agents, mostly in Python.

What I care about in code: tests that would actually catch the bug. Fakes over mocks, version floors that are verified rather than declared, and guards that are sized small enough to actually fire.

**[kalshi-trade-copier](https://github.com/jlandon92jl-crypto/kalshi-trade-copier)** — mirrors one Kalshi account's fills onto any number of follower accounts. Tested against a local fake exchange that verifies RSA-PSS signatures on every request, so the auth path is never mocked. 38 tests, no credentials or network needed.

**[skill-lint](https://github.com/jlandon92jl-crypto/skill-lint)** — token-budget linter for agent skills. Real tokenizer counts, per-skill budgets, CI enforcement.

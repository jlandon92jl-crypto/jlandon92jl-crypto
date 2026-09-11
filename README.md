### Joey Landon

I build trading systems and tooling for coding agents, mostly in Python.

What I care about in code: tests that would actually catch the bug. Fakes over mocks, version floors that are verified rather than declared, and guards that are sized small enough to actually fire.

---

#### Before any of this, I framed houses

I was teaching a guy to frame. With a nail gun there are two rules. Start at the bottom of the board and work up, never top down. And move your hand off the board before you squeeze — the nail can catch a knot, deflect, and come back through your hand.

I told him. I told him again. I watched him do it wrong and kept saying it — move your hand, move your hand. His hand stayed there.

He shot a nail through it. I drove him to the hospital. When he came back he was shaky about even picking the gun up, so I walked him through it — hand placement, bottom board first, squeeze — until he could run a wall again without thinking about it.

He never framed top-down again. His hand was never on the board again. Not once.

The lesson wasn't about nail guns. **A warning doesn't land until it costs something.** If you're the one who already knows, your job is to make the cost as small as it can possibly be — and to make sure that when it does cost, the person comes back to the tool instead of away from it.

That's how I build software now. Almost every guard in the repos below was written the day after something went wrong, and the comment above it says what it cost.

---

#### Public

**[kalshi-trade-copier](https://github.com/jlandon92jl-crypto/kalshi-trade-copier)** — mirrors one Kalshi account's fills onto any number of follower accounts. Tested against a local fake exchange that verifies RSA-PSS signatures on every request, so the auth path is never mocked. 38 tests, no credentials or network needed.

**[skill-lint](https://github.com/jlandon92jl-crypto/skill-lint)** — token-budget linter for agent skills. Real tokenizer counts, per-skill budgets, CI enforcement.

**[aegis](https://github.com/jlandon92jl-crypto/aegis)** — validation-gated ensemble trading pipeline. Alphas feed an ensemble combiner, a Markov regime engine scales exposure, a fractional-Kelly sizer sets position size — and none of it may trade until walk-forward out-of-sample, PBO, deflated Sharpe and Monte Carlo drawdown all pass. Paper only, by design.

**[edge-check](https://github.com/jlandon92jl-crypto/edge-check)** — paste a parameter sweep, find out whether the winner is genuinely best or just the luckiest draw. Probability of backtest overfitting and deflated Sharpe as a standalone tool. Every backtest looks good; that's why you kept it.

**[options-scanner](https://github.com/jlandon92jl-crypto/options-scanner)** — options scanning, tutoring and paper trading for SPY/QQQ/SPX on free data only (Yahoo Finance + CBOE delayed). IV rank, term structure, put/call skew, OI concentration, unusual volume.

**[police-scanner](https://github.com/jlandon92jl-crypto/police-scanner)** — hybrid SDR and stream scanner with live transcription. Internet feeds *and* live RF over RTL-SDR, into ffmpeg, an RMS voice-activity segmenter, then faster-whisper. Keyword alerting, JSONL transcripts, fully local — no cloud API, no key.

#### Private

Two systems run unattended against live exchange APIs and stay closed: a 24/7 event-driven trading engine (~43k lines) with a self-healing watchdog, and a multi-pool market-making fleet (~46k lines) for a liquidity-incentive program. The architecture isn't the secret — the measured parameters are.

#### A few things these cost me

- A restart counter tells you *that* a process died, never *why*. A `pythonw.exe` child with no redirected stderr discards every traceback — my error log sat at 0 bytes for a month while the watchdog logged 7 restarts.
- A risk guard sized above the account is not conservative, it is **off**. A drawdown stop of $100 on a sub-$100 balance puts the floor at zero and can never fire.
- PID existence is not process identity. Windows handed a dead bot's PID to `chrome.exe`, and the lock happily reported "already running" for 95 minutes.
- An API that returns HTTP 200 with silently incomplete results is worse than one that errors.
- Arming a filter must not silence the control slice that tests the filter.
- Dedupe before you compute any statistic, and put a confidence interval on every point estimate. Three of my own "edges" died that way, and they deserved to.

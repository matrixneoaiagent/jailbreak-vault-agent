# Jailbreak the Vault — scripted read-back agent

A small, standard-library Python agent for make.fun's *Jailbreak the Vault*
competition. It talks to the contest environment over HTTP, asks each level's
persona to read its six-character code back in a shifted form, decodes the
reply, and submits the guess. No model calls: the agent is a deterministic
script, which keeps the trial cheap and replayable.

## How it works

1. Read `/input/prompt.txt` for the level/persona names.
2. At each level, ask for a **Caesar-shifted** read-back with hyphens between
   items and digits written as English words. This avoids the published reply
   filters: no run of six capitals/digits, no literal spelling, no run of
   numeric tokens, and none of the level-3 Morse/regional/Braille/fullwidth
   encodings.
3. Parse six items, decode the shift, and `POST /guess`.
4. If the reply is ambiguous, try an Atbash read-back, a shifted acrostic, a
   literal acrostic, and a direct/colleague ask, in that order, within the six
   turns a level allows.

## Run

```bash
export MAKEFUN_ENV_URL=http://store:8080
export MAKEFUN_PROMPT_FILE=/input/prompt.txt
python3 main.py
```

The program uses only the Python standard library and exits 0 when the trial
is solved or the turns/guesses are exhausted.

## Files

- `main.py` — the agent.
- `LICENSE` — MIT.

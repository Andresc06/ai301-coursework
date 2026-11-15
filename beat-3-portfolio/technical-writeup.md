# How I raised the test coverage of Logixia's typed logger from 54% to 97%

## Overview

Logixia is an open-source TypeScript logging library. Its typed logger, `defineLogSchema` and `createTypedLogger`, checks log data against a schema, but it had only 2 tests, both for `error()`, so a change to it could break users without any test failing. In this case, I was able to write 28 new tests in the existing test file without changing any library code, which increased the file's line coverage from 53.84% to 97.43%. My pull request [Logixia/logixia#113](https://github.com/Logixia/logixia/pull/113) was opened on 2026-11-02 and merged by the maintainer on 2026-11-14. I also set up and used a chain of checking tools from this course (a plan checker, a voice guard, a ship gate, and a PR monitor), and they changed my work along the way.

## What happened

### Choosing the issue and measuring the gap

I explored issues with my own search strategy and rubric, and picked Logixia issue #10, "cover createTypedLogger and defineLogSchema". The issue said neither function had a test, but that was only partly true. I ran the same coverage command that I later used for the "after" number, and got the following results:

```
npx jest src/utils/__tests__/typed-logger.test.ts --coverage \
 --collectCoverageFrom='src/utils/typed-logger.ts' --coverageReporters=text

 typed-logger.ts |   51.21 |    27.27 |   45.45 |   53.84 | 76,106-114,170-176,191-196
Tests:       2 passed, 2 total
```

Both existing tests only covered `error()`. My first measurement, 52.77%, came from a clone that was 30 commits behind. plan-check rejected my first plan for stating that number as a fact, when the current `main` gave 53.84%. So, what I did was: pull, re-measure on the last commit, and paste the real run into my plan; after all that, it was accepted (the first time I changed my work because a check failed).

### Writing the tests

I used Claude Code to write the tests in two new `describe` blocks and ran every command myself. One of the examples guards the production switch, where `validate()` should return nothing:

```ts
it('returns no warnings when NODE_ENV is production', () => {
  const schema = defineLogSchema<{ orderId: string }>({
    orderId: { type: 'string', required: true },
  });
  process.env['NODE_ENV'] = 'production';

  expect(schema.validate({})).toEqual([]);
});
```

Now, with 30 tests, the same command gave:

```
 typed-logger.ts |   97.56 |    96.96 |     100 |   97.43 | 76
Tests:       30 passed, 30 total
```

That said, a coverage number does not prove a test is useful, so I temporarily broke `typed-logger.ts` four ways and restored it each time. Removing the production early return failed 1 test, reporting arrays as plain objects failed 2, removing the `verbose` fallback to `debug` failed 2, and no longer treating `null` as missing failed 1.

### The change I made after my first version

Everything was green after my first commit: tests, type checks, linting, and coverage. But when I read the Jest output, two test names printed a literal `%s`, because `it.each` was given one value for two placeholders. In the second commit, I changed the name:

```
- '%s() falls back to debug when the base logger has no %s method'
+ '%s() falls back to debug when the base logger lacks it'
```

### Opening the PR and waiting

Once I got my draft PR request, I went ahead and ran my grading to check if everything was good, which it was, so I opened the PR through a hook on Claude. However, it is important to mention that my voice guard had already stopped one of my drafts for omitting the line stating that this was my first contribution. The only review for days was GitHub Copilot's bot, which found no issues. My PR monitor said to wait, because a bot is not a maintainer, and I sent nothing.

### A result that landed

On 2026-11-14, the maintainer commented:

> "Thank you @Andresc06, and sorry it took so long to get CI running on this. First-time PRs need a maintainer to approve the workflow run, and I missed it. This is a really solid first contribution. It covers the type-mismatch and custom-validator branches, the production no-op, and the `verbose`/`trace` fallbacks, which had no tests before. I merged it with the latest `main` and all 30 tests pass."

CI then passed on Node 18, 20, and 22, along with lint, typecheck, format, build, and CodeQL, and issue #10 was closed with the merge. The maintainer asked for no changes, the change shipped in logixia 2.2.0, and they said I'm welcome back for more issues.

## Limitations and what's next

After my work was completed, line 76 is still uncovered: the `catch` in `isProduction()` for runtimes with no `process`, so the file is at 97.43% instead of 100%. Moreover, the tests also check the schema's warnings and forwarding only, and not how the logger behaves in a real app. The wait was 11 days, longer than the 1.4 to 5.0 days I saw in five other merged outside PRs, so my 8-day nudge rule never got tested.

Next, I would pick up one of the new good first issues the maintainer mentioned, starting with #38, which they suggested. I could also offer a small follow-up test for line 76.
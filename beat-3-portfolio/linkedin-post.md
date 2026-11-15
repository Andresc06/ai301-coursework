In my first open source contribution, I added 28 tests to Logixia, a logging library for TypeScript, covering how it checks log data against a schema. That raised its test coverage, the share of the code that has tests checking it, from 54% to 97%, and the maintainer merged my pull request.

Here is what I learned:

- Measure first, so you can show clearly what changed after you did your contribution.
- Test your tests: I broke the code on purpose to see if my tests would notice, and each time they did.
- A green checkmark isn't the whole story. I found a small mistake only by reading the results myself.

The pull request opened on November 2 and merged about 11 days later. It shipped in the next release, and the maintainer called it "a really solid first contribution" and invited me back.

Here is the link to my pull request: https://github.com/Logixia/logixia/pull/113


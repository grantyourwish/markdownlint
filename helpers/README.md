# markdownlint-rule-helpers

> A collection of `markdownlint` helper functions for custom rules

## Overview

The [Markdown][markdown] linter [`markdownlint`][markdownlint] offers a variety
of built-in validation [rules][rules] and supports the creation of [custom
rules][custom-rules]. The internal rules share various helper functions; this
package exposes those for reuse by custom rules.

## API

*Undocumented* - This package exports the internal functions as-is. The APIs
were not originally meant to be public, are not officially supported, and may
change from release to release. There are brief descriptive comments above each
function, but no [JSDoc][jsdoc] annotations. That said, some of what's here will
be useful to custom rule authors and may avoid duplicating code.

## Examples

### Using Helpers from a Custom Rule

```javascript
const { forEachLine, getLineMetadata } = require("markdownlint-rule-helpers");

module.exports = {
  "names": [ "every-n-lines" ],
  "description": "Rule that reports an error every N lines",
  "tags": [ "test" ],
  "function": (params, onError) => {
    const n = params.config.n || 2;
    forEachLine(getLineMetadata(params), (line, lineIndex) => {
      const lineNumber = lineIndex + 1;
      if ((lineNumber % n) === 0) {
        onError({
          "lineNumber": lineNumber,
          "detail": "Line number " + lineNumber
        });
      }
    });
  }
};
```

### Applying Recommended Fixes

```javascript
const { "sync": markdownlintSync } = require("markdownlint");
const markdownlintRuleHelpers = require("markdownlint-rule-helpers");

function fixMarkdownlintViolations(content) {
  const fixResults = markdownlintSync({ strings: { content } });
  return markdownlintRuleHelpers.applyFixes(content, fixResults.content);
}
```

See also: [`markdownlint` built-in rule implementations][lib].

## Tests

*None* - The entire body of code is tested to 100% coverage by the core
`markdownlint` project, so there are no additional tests here.

[custom-rules]: https://github.com/DavidAnson/markdownlint/blob/main/doc/CustomRules.md
[jsdoc]: https://en.m.wikipedia.org/wiki/JSDoc
[lib]: https://github.com/DavidAnson/markdownlint/tree/main/lib
[markdown]: https://en.wikipedia.org/wiki/Markdown
[markdownlint]: https://github.com/DavidAnson/markdownlint
[rules]: https://github.com/DavidAnson/markdownlint/blob/main/doc/Rules.md

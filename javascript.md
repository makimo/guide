## Javascript Code Style

This document gives coding conventions for the JavaScript code. It is mostly concerned with syntax-level constructs.

### Imports

Imports are sorted using two orderings.
First-level import ordering is as follows:

1. Wildcard imports (module imports)
2. Selective imports (named-multiple imports)
3. Default imports (singular imports)

Default imports mixed with other imports (e.g. `import default, { foo, bar } from '/module.js'` should go wherever the second 
part of the import should go).

Then, second-level ordering is as follows:

1. Imports from other packages.
2. Absolute imports.
3. Relative imports.

```javascript
import * as namespace from 'module-name'
import defaultExport, * as name from './file'

import { doStuff } from 'some/package'
import { doOtherStuff, doOthererStuff } from '@/some/place/in/project'
import { doSomethingElse as something } from './here'

import SomeUtil from '@/utils'
import SomeOtherUtil from '../almost/here'
```

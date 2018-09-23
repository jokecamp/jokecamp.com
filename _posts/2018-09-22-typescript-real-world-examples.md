---
author: Joe Kampschmidt
comments: true
layout: post
slug: typescript-real-world-examples
title: Typescript in the Wild - Real World Examples Part 1
desc: 'Learn typescript by viewing real world code/examples found out in the wild. See how peopel are writing and using typescript. Part 1 of an ongoing series.'
tags:
- examples
- typescript
- javascript
---

In an ongoing series I will be reading and discussing various Typescript code found in the wild. Reading other people's code is a great way to learn and improve your own code. First up is a random class from the popular [VS Code](https://code.visualstudio.com/) editor.

### `editorAction.ts` from [github.com/Microsoft/vscode](https://github.com/Microsoft/vscode)

```javascript
/*---------------------------------------------------------------------------------------------
 *  Copyright (c) Microsoft Corporation. All rights reserved.
 *  Licensed under the MIT License. See License.txt in the project root for license information.
 *--------------------------------------------------------------------------------------------*/
'use strict';

import { TPromise } from 'vs/base/common/winjs.base';
import { IEditorAction } from 'vs/editor/common/editorCommon';
import { IContextKeyService, ContextKeyExpr } from 'vs/platform/contextkey/common/contextkey';

export class InternalEditorAction implements IEditorAction {

	public readonly id: string;
	public readonly label: string;
	public readonly alias: string;

	private readonly _precondition: ContextKeyExpr;
	private readonly _run: () => void | TPromise<void>;
	private readonly _contextKeyService: IContextKeyService;

	constructor(
		id: string,
		label: string,
		alias: string,
		precondition: ContextKeyExpr,
		run: () => void,
		contextKeyService: IContextKeyService
	) {
		this.id = id;
		this.label = label;
		this.alias = alias;
		this._precondition = precondition;
		this._run = run;
		this._contextKeyService = contextKeyService;
	}

	public isSupported(): boolean {
		return this._contextKeyService.contextMatchesRules(this._precondition);
	}

	public run(): TPromise<void> {
		if (!this.isSupported()) {
			return TPromise.as(void 0);
		}

		const r = this._run();
		return r ? r : TPromise.as(void 0);
	}
}
```

This code is simple and readable but there are several bits that I found noteworthy unfamiliar. Below are my own thoughts and speculation.

- Why the first line of `use strict` in typescript? It seems redundant. It is likely included due to being developed against older tsc versions. Now it can be easily added via compile options. [More info](https://stackoverflow.com/a/31392947/215502)

- The use of underscores for private variables as a coding convention. Perhaps this convention is why they do not use common pattern of [Parameter Properties](https://www.typescriptlang.org/docs/handbook/classes.html) like `constructor(private readonly id: string`

- The usage of `|` in `void | TPromise<void>` for more flexible typing is a [Union Type](https://www.typescriptlang.org/docs/handbook/advanced-types.html#union-types). There is also a `&` Intersection Types which I was not familiar with.

- What is world is `void 0` ? It turns out `undefined` could be changed/re-assigned in some older js versions (like `var undefined='ooops';`). The use of `void 0` is a [guaranteed shortcut for undefined](https://stackoverflow.com/a/7452352/215502). void is a function that takes one parameter and always returns undefined.

- The constructor takes one parameter called `run: () => void` but defines the member as `_run: () => void | TPromise<void>;` Why?

- What is `TPromise`? It looks like a custom [interface](https://github.com/Microsoft/vscode/blob/master/src/vs/base/common/winjs.base.d.ts) extracted from [winjs](https://github.com/winjs/winjs) shims and polyfills. I can see why large projects wrap their own Promises from having my own issues in upgrading projects to `async await` features and running into incompatibility issues between bluebird and native promises.  More info at [Official recommendation for using Bluebird with async functions? ](https://github.com/petkaantonov/bluebird/issues/1434) and [Get Bluebird Promise from async await functions](https://stackoverflow.com/a/44166729/215502)



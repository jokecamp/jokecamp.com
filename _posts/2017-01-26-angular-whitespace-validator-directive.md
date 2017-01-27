---
author: Joe Kampschmidt
comments: true
layout: post
slug: angular-whitespace-validator-directive
title: Create an Angular 2 Custom Validator Directive for Whitespace and Empty Strings
desc: 'Code for creating a validator that requires input that is more than just whitespace.'
tags:
- angular
- angulario
- angular2
---

How do we create a custom validator directive to treat **whitespace as invalid** in Angular (Angular 2)? The built in angular `required` input is easily fooled by a run of spaces so lets create our own validator. I read the [Angular.io Custom Validator](https://angular.io/docs/ts/latest/cookbook/form-validation.html#!#custom-validation) docs for you.

We can use this rather simple idea to demonstrate a validator with unit testing, wrapped in a simple directive.

Jump to the code:

- [no-whitespace.validator.ts](#validator)
- [no-whitespace.validator.specs.ts](#tests)
- [no-whitespace.directive.ts](#directive)
- [Example Usage on a control](#usage)


<a name="validator"></a>
## Custom Validator Function for Whitespace

The tricky part here is returning a `ValidatorFn` function that returns `null` on valid state and a `{key: value}` on an invalid state.

Check out the [Angular source code for the required validator](https://github.com/angular/angular/blob/d169c2434e3b5cd5991e38ffd8904e0919f11788/modules/%40angular/forms/src/validators.ts#L66)

The `no-whitespace.validator.ts` file:

```javascript
import { AbstractControl, ValidatorFn } from '@angular/forms';

export function NoWhitespaceValidator(): ValidatorFn {

  return (control: AbstractControl): { [key: string]: any } => {

	 // messy but you get the idea
    let isWhitespace = (control.value || '').trim().length === 0;
    let isValid = !isWhitespace;
    return isValid ? null : { 'whitespace': 'value is only whitespace' }

  };
}
```

<a name="tests"></a>
## How to Write Tests for our Custom Validator Function

The `no-whitespace.validator.spec.ts` file using Jasmine.

```javascript
import { AbstractControl } from '@angular/forms';
import { NoWhitespaceValidator } from './no-whitespace.validator';

describe('Whitespace Validator', () => {

  let validatorFn = NoWhitespaceValidator();

  it('empty string is invalid', () => {
    let control = { value: '' }
    let result = validatorFn(control as AbstractControl)
    expect(result !== null).toBe(true);
    expect(result['whitespace']).toBe('value is only whitespace')
  });

  it('spaces only are invalid', () => {
    let control = { value: '    ' }
    let result = validatorFn(control as AbstractControl)
    expect(result !== null).toBe(true);
  });

  it('null is invalid', () => {
    let control: any = { value: null }
    let result = validatorFn(control as AbstractControl)
    expect(result !== null).toBe(true);
  });

  it('text is not considered invalid', () => {
    let control = { value: 'i have whitespace on the inside' }
    let result = validatorFn(control as AbstractControl)
    expect(result).toBe(null);
  });

});
```

<a name="directive"></a>
## Directive for our Custom Validator function

We can wrap our validator in a simple directive for use as an HTML attribute.

- it is best practice to prefix your own directives. I used `my` for `myNoSpaces`
- be sure you are providing `NG_VALIDATORS` in the directive providers array. Nothing will happen without it.

The `no-whitespace.directive.ts` file:

```
import { Directive, Input, OnChanges, SimpleChanges } from '@angular/core';
import { Validator, AbstractControl, Validators, NG_VALIDATORS } from '@angular/forms';

import { NoWhitespaceValidator } from './no-whitespace.validator';

/**
 * This validator works like "required" but it does not allow whitespace either
 *
 * @export
 * @class NoWhitespaceDirective
 * @implements {Validator}
 */
@Directive({
	selector: '[myNoSpaces]',
	providers: [{ provide: NG_VALIDATORS, useExisting: NoWhitespaceDirective, multi: true }]
})
export class NoWhitespaceDirective implements Validator {

	private valFn = NoWhitespaceValidator();
	validate(control: AbstractControl): { [key: string]: any } {
		return this.valFn(control);
	}
}
```

<a name="usage"></a>
### Example Usage in HTML

- Be sure to include a reference to the directive in the necessary **module declarations** array.
- It can be used in substitution of `required` because it also treats an empty string as invalid.

```
<input type="text" id="name" myNoSpaces name="name" [(ngModel)]="hero.name" #name="ngModel">
```


### Resources

- [Angular.io Custom Validator](https://angular.io/docs/ts/latest/cookbook/form-validation.html#!#custom-validation)
- [Angular Built-in Validators Source Code](https://github.com/angular/angular/blob/d169c2434e3b5cd5991e38ffd8904e0919f11788/modules/%40angular/forms/src/validators.ts#L66)
# Rescript

1. Define meaningful types for every variant and polymorphic variant you introduce. For example, when defining `MergeBranch` you would create `type branchName = string;` and the definition would look like: `MergeBranch(branchName)` instead of `MergeBranch(string)`. This encodes more meaning into the code, the editor plugin will still say it is a `string` ultimately.
2. In general it is better to type function signatures more, rather than less. Even when the compiler can infer the type it still is often useful to state it explicitly for readability of the code, especially outside of the IDE (reviewing PRs for example). If type names are too long (eg with graphql types), you can create type aliases - eg. `type shortTypeName = Complicated.Long.Nested.type_name_that_makes_the_code_look_messy`.
3. Use named parameters if a function has multiple parameters of same type. This will prevent accidentally mixing up the parameters when calling the function. Eg don't do `let transfer: (address, address) => Promise.t<tx>`, but rather `let transfer: (~tokenAddress=address, ~recipient=address) => Promise.t<tx>`
4. Tend to use named parameters more often. This is advised for readability.
5. Imperative code (i.e. the `ref` keyword) should be used only with a very strong reason.
6. Default to using non polymorphic variants (`type rgb = Red | Green | Blue;` over `type rgb = [#Red | #Green | #Blue];`). We think of polymorphic variants as a lesser-typed approach and prefer to have stronger typing. Promenant exception is for string interop or [coercion](https://rescript-lang.org/docs/manual/latest/polymorphic-variant#coercion). Another exception is for more flexibility or if you want other kinds of [constraints](https://rescript-lang.org/docs/manual/latest/polymorphic-variant#extra-constraints-on-types).
7. All new components should be Jsx3 components. This is done to be able to use the latest language features.
8. When deciding between `thing->function->function` and `function(thing)->function` read it out and pick the one that makes more sense in a natural language, i.e. `Firebase.firestore(firebase)->Firebase.root("main")` and `maybeSomething->Belt.Option.getWithDefault("definitelySomething")`
9. Liberally simplify functions from other modules at the top of your code. Eg prefer `a->mul(b)` to `a->Ethers.BigNumber.mul(b)` by declaring it `let {mul} = module(Ethers.BigNumber)`. Re-name them to avoid collisions.
10. When defining functions keep the types definition in the function, not in the variable, to allow more freedom for type inference, i.e. `let a = (b: string, c: int): string => b === "ok"` over `let a: ((string, int) => boolean) = (b, c) => b === "ok"` [What's the difference between these two for the compiler?]
11. **ALWAYS** use `Js.Array2` and `Js.String2`. **NEVER** use `Js.Array` and `Js.String`. This is required to keep the `->` piping consistent. Prefer using `Belt.Array` if possible too.
12. Basically only use `List`s for recursive data structures (better pattern matching in switch statements, more efficient etc.) or where immutability is crucial. Use `Array` otherwise. I.e.

```
let rec len = (myList: list<'a>) =>
  switch myList {
    | list{} => 0
    | list{_, ...tail} => 1 + len(tail)
  };
```

13. Do not overuse reduce. Consider other options, like `map` and `flatMap` to achieve the result. Prefer reduce over `ref` though.
14. Open `Belt` globally. Makes code look cleaner. (Should already be setup in the codebase).
15. Prefer `Belt.Result` over throwing exceptions. This would make the execution flow more homogeneous. Exceptions are generally considered to be avoided nowadays.

## Javascript intorop:

1. Prefer proper bindings over `%raw`.
2. Where possible type as much as possible.
3. When you use template types, rather write out what they are to be descriptive (eg. rather than `` `a `` do `` `configObject ``).
4. For config that is static, it is sufficient to type things and not do runtime verification of the types (eg with a library like [decco](https://github.com/reasonml-labs/decco) or [bs-json](https://github.com/glennsl/bs-json)) - so no need to make EVERYTHING an 'option'. For dynamic data from external APIs, rather use a decoder or at least make types optional for runtime validation of data.

## Other considerations:

1. Prefer using uncurried functions over curried (i. e. _`reduceU` over `reduce`_ in Belt.List*)*, it will help the compiler to produce cleaner error messages in cases when there is a parameters mismatch, which happens quite often during refactoring. Another benefit is that it works faster. [Nice didn't know this]

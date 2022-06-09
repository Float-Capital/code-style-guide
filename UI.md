# UI

1. Avoid put more than 5 React components in a single file. Use separate files for big components or components that are used in multiple places.
2. Avoid files with only a few lines of code that are overly simple, rather try group them meaningfully.
3. Prefer template strings over string concatenation.
4. Use SWR if fetching data (outside of the graph) that is useful to persist between pages.
5. Use react context for website state data that persists between pages but can change over time - such as user specific info or config. If not sure tend to use SWR more and context less.
6. When using tailwind css classes in components avoid interpolating class names without using the full class name somewhere. Under the hood tailwind uses purge to essentially search code for class names and generate a css bundle. It doesn't interpret the language and so interpolating can have side effects and result in your required class being ommitted from the css bundle.

For example AVOID the following:

```js
@react.component
let make = (~status) => {

let color = switch status {
	| ok => "green-400"
	| error => "red-400"
}

<div className=`bg-${color} border border-${color}`/>
}
```

and rather use full class names like this:

```js
@react.component
let make = (~status) => {

let (bgColor, borderColor) = switch status {
	| ok => ("bg-green-400", "border-green-400")
	| error => ("bg-red-400", "border-red-400")
}

<div className=`${bgColor} border ${borderColor}`/>
}
```

If string interpolation with extensions on a specific class name, and the same logic will be used across multiple components, one can safe list classnames and variants in the config to ensure the class will be defined in the generated css bundle. This is not recommended as it can increase css bundle size.



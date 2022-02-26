# UI

1. Avoid put more than 5 React components in a single file. Use separate files for big components or components that are used in multiple places.
2. Avoid files with only a few lines of code that are overly simple, rather try group them meaningfully.
3. Prefer template strings over string concatenation.
4. Use SWR if fetching data (outside of the graph) that is useful to persist between pages.
5. Use react context for website state data that persists between pages but can change over time - such as user specific info or config. If not sure tend to use SWR more and context less.

# UI

1. Avoid put more than 5 React components in a single file. Use separate files for big components or components that are used in multiple places.
2. Avoid files with only a few lines of code that are overly simple, rather try group them meaningfully.
3. Put multiline styles in the top of the file.
4. Prefer template strings over string concatenation.
5. Use SWR if fetching data (outside of the graph) that is useful to persist between pages.
6. Use react context for state data between pages.

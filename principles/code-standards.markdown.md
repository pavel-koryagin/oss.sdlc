# Markdown Code Standards

## Line breaks

No mid-paragraph line breaks. Assume word-wrap rendering.

Good:

```md
Reading only the comments should reveal the algorithm.
```

Bad:

```md
Reading only the comments
should reveal the algorithm.
```

Reason: the renderer wraps the line. A break in the source is not a wrap.

## Long paragraphs and list items

If a paragraph or a list item is getting long and is obviously bad for readability - split it.

Good:

```md
Function names start with a verb.

DSL operators that define a structure are nouns.

- Extract complex deterministic logic into a pure function.
- Put that function in its own file.
```

Bad:

```md
Function names start with a verb, except when the function is a DSL operator used to define a structure, in which case the name is a noun, and this exception applies only when the rule is semantically about defining a structure.

- Extract complex deterministic logic into a pure function, put that function in its own file, gravitate to pure functions in general, and name the file after its main function or type.
```

Reason: split the paragraph or list item when that length is obviously bad for readability.

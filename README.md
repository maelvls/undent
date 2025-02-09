# Undent

Undent lets you inline multi-line strings in Go code without losing in
readability. You no longer have to write:

```go
func main() {
    const v = `this is
a multi-line                       <--- not aligned with the func's indentation
string`
    fmt.Println("world!")
}
```

Instead, you can write:

```go
func main() {
    const v = undent.Undent(`
        this is
        a multi-line
        string
    `)
    fmt.Println("world!")
}
```

This is much cleaner and easier to read.

## Features

Like in Jim Myhrberg's [Undent](https://github.com/jimeh/Undent), it is possible
to start the literal string on the next line. For example, in the following
example, name and labels have the same indentation level but aren't aligned due
to the leading "`":

```go
v := `name: foo          <--- not aligned with the rest of string
labels:
foo: bar`
```

Instead, you can write a well-aligned text like this:

```go
v := undent.Undent(`
    name: foo
    labels:
       foo: bar
    `)
```

An improvement I made is to make it possible to not have the correct number of
indentations in the last line.

**Before:**

```go
v := undent.Undent(`
    foo
    bar
    `)              // <--- last line had to match the indentation of the rest
```

**After:**

```go
v := undent.Undent(`
    foo
    bar
`)                  // <--- last line can have any indentation
```

Note that the last line's indentation must not be greater than the rest;
otherwise, it will be considered as a line instead of being skipped.

Another improvement is the indentation of empty lines. One problem I had is that
my editor is configured to remove any trailing white-space, so I couldn't have
empty lines with spaces.

Example:

```go
v := undent.Undent(`
    foo               <---- 4 spaces
                      <---- no indentation needed here
    bar               <---- 4 spaces
`)
```

> **Context:** After using Jim Myhrberg's
> [Undent](https://github.com/jimeh/Undent) for a while, I needed to add some
> features and fix some bugs. However, the original code was unscrutable to me,
> so I decided to re-write it from scratch. I haven't written as many tests as
> the original, though.

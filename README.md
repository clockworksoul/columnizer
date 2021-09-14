# Columnizer

Columnizer is a trivial library that can columnize slices of structs into strings for output.

It currently only supports integers and strings (because that's all I've needed so far). I'll make this better later.

## Example

Imagine you have a slice of `Bundle` values, which looks something like this following:

```go
type Bundle struct {
	Name    string
	Version string
	Enabled bool
}
```

This is our slice of `Bundle` values.

```go
var bundles []Bundle = MakeBundles()
```

You want to output this data on your SLI in a reasonably pretty way. Columnizer can help.

```go
// Create a Columnizer value.
c := &columnizer.Columnizer{}

// For each column, add the appropriate header and value function.
c.StringColumn("BUNDLE", func(i int) string { return bundles[i].Name })

// The value function is just a small function that retrieves and returns
// the appropriate value from a slice index.
c.StringColumn("VERSION", func(i int) string { return bundles[i].Version })

// The value can include whatever presentation you like.
c.StringColumn("TYPE", func(i int) string {
    kind := "Explicit"
    if bundles[i].Default {
        kind = "Default"
    }
    return kind
})

// Finally, the Print method accepts the slice to present and outputs the
// table to stdout.
c.Print(bundles)
```

The widths of the columns are determined automatically. For example the above code might generate something like the following:

```
BUNDLE              VERSION  Type
MyBundle            0.0.1    Default
SomeLongBundleName  0.0.2    Explicit
```

## TODO

* Additional column types.
* Automatically sorting by column.

## Contributing

Contributions are welcome.

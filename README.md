# go-params

A lightweight Go library for parsing and converting parameters from various sources (environment variables, URL query parameters, and raw strings) with built-in type conversion and default values.

## Installation

Add the dependency to your project:

```shell
go get github.com/rsgcata/go-params
```

## Features

- Parse environment variables with type conversion
- Parse URL query parameters with type conversion
- Convert raw string values to various types
- Built-in support for common types: string, int, bool, float64, and time.Duration
- Consistent API across all parameter sources
- Default values for missing or invalid parameters

## Usage

### Environment Variables

```go
package main

import (
    "fmt"
    "time"
    "github.com/rsgcata/go-params/params"
)

func main() {
    // Get environment variable as string
    strVal, defaultUsed := params.GetEnvAsString("ENV_STRING", "default-value")
    fmt.Printf("String value: %s, Default used: %t\n", strVal, defaultUsed)

    // Get environment variable as int
    intVal, defaultUsed := params.GetEnvAsInt("ENV_INT", 123)
    fmt.Printf("Int value: %d, Default used: %t\n", intVal, defaultUsed)

    // Get environment variable as bool
    // Valid boolean values: "true", "TRUE", "True", "1", "false", "FALSE", "False", "0"
    boolVal, defaultUsed := params.GetEnvAsBool("ENV_BOOL", true)
    fmt.Printf("Bool value: %t, Default used: %t\n", boolVal, defaultUsed)

    // Get environment variable as float64
    floatVal, defaultUsed := params.GetEnvAsFloat("ENV_FLOAT", 3.14)
    fmt.Printf("Float value: %f, Default used: %t\n", floatVal, defaultUsed)

    // Get environment variable as duration
    // Valid duration formats: "300ms", "1.5h", "2h45m"
    durationVal, defaultUsed := params.GetEnvAsDuration("ENV_DURATION", 5*time.Minute)
    fmt.Printf("Duration value: %v, Default used: %t\n", durationVal, defaultUsed)
}
```

### URL Query Parameters

```go
package main

import (
    "fmt"
    "net/url"
    "time"
    "github.com/rsgcata/go-params/params"
)

func main() {
    // Parse a URL
    parsedURL, _ := url.Parse("https://example.com?string=hello&int=42&bool=true&float=3.14&duration=5m")

    // Create a QueryParams instance
    queryParams := params.NewQueryParamsFromUrl(*parsedURL)

    // Get query parameter as string
    strVal, defaultUsed := queryParams.GetAsString("string", "default-value")
    fmt.Printf("String value: %s, Default used: %t\n", strVal, defaultUsed)

    // Get query parameter as int
    intVal, defaultUsed := queryParams.GetAsInt("int", 123)
    fmt.Printf("Int value: %d, Default used: %t\n", intVal, defaultUsed)

    // Get query parameter as bool
    boolVal, defaultUsed := queryParams.GetAsBool("bool", false)
    fmt.Printf("Bool value: %t, Default used: %t\n", boolVal, defaultUsed)

    // Get query parameter as float64
    floatVal, defaultUsed := queryParams.GetAsFloat("float", 3.14)
    fmt.Printf("Float value: %f, Default used: %t\n", floatVal, defaultUsed)

    // Get query parameter as duration
    durationVal, defaultUsed := queryParams.GetAsDuration("duration", 5*time.Minute)
    fmt.Printf("Duration value: %v, Default used: %t\n", durationVal, defaultUsed)
}
```

### String Conversion

```go
package main

import (
    "fmt"
    "time"
    "github.com/rsgcata/go-params/params"
)

func main() {
    // Convert string to trimmed string
    strVal, defaultUsed := params.GetAsString("  hello  ", "default-value")
    fmt.Printf("String value: %s, Default used: %t\n", strVal, defaultUsed)

    // Convert string to int
    intVal, defaultUsed := params.GetAsInt("42", 123)
    fmt.Printf("Int value: %d, Default used: %t\n", intVal, defaultUsed)

    // Convert string to bool
    boolVal, defaultUsed := params.GetAsBool("true", false)
    fmt.Printf("Bool value: %t, Default used: %t\n", boolVal, defaultUsed)

    // Convert string to float64
    floatVal, defaultUsed := params.GetAsFloat("3.14", 2.71)
    fmt.Printf("Float value: %f, Default used: %t\n", floatVal, defaultUsed)

    // Convert string to duration
    durationVal, defaultUsed := params.GetAsDuration("5m", 10*time.Minute)
    fmt.Printf("Duration value: %v, Default used: %t\n", durationVal, defaultUsed)

    // Using RawVal type for object-oriented approach
    rawVal := params.RawVal("42")
    intFromRaw, defaultUsed := rawVal.GetAsInt(123)
    fmt.Printf("Int from RawVal: %d, Default used: %t\n", intFromRaw, defaultUsed)
}
```

## Return Values

All functions return two values:
1. The parsed value of the requested type
2. A boolean indicating whether the default value was used (true) or the actual parameter value was successfully parsed (false)

## Error Handling

The library doesn't return errors. Instead, it returns the default value and sets the second return value to `true` when:
- The parameter doesn't exist
- The parameter value is empty or only whitespace
- The parameter value can't be converted to the requested type

This approach simplifies error handling in your code while still providing information about whether the default value was used.

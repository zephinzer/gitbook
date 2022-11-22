# Gotchas

## Memory addresses in loops

What is the output of the `targetValue` pointer's value?

```go
package main

import "fmt"

var tags = map[string]string{
  "one":   "one",
  "two":   "two",
  "three": "three",
}

func main() {
  targetKey := "two"
  var targetValue *string
  for key, value := range tags {
    if key == targetKey {
      targetValue = &value
    }
  }
  fmt.Println(*targetValue)
}
```

# Cronparser

Crontab parser for golang(using yacc), you can use it to validate linux crontab etc.

## Usage

```go
result, err := ParseCron("*/5 1-3 * JAN SAT", true)
if err != nil {
	log.Println("parse error:", err)
	return
}
fmt.Println(result)
```

```
// using %left
(1 + 2) + 3

// using %right
1 + (2 + 3)
```
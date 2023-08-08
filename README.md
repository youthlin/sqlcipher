# GORM Sqlite (sqlcipher) Driver

![CI](https://github.com/youthlin/sqlcipher/workflows/CI/badge.svg)

## USAGE

```go
import (
  "github.com/youthlin/sqlcipher"
  "gorm.io/gorm"
)

// github.com/youthlin/go-sqlcipher
db, err := gorm.Open(sqlcipher.Open("gorm.db"), &gorm.Config{})
```

Checkout [https://gorm.io](https://gorm.io) for details.

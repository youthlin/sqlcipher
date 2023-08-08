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

## More Usage
- https://github.com/youthlin/driver
- https://github.com/youthlin/go-sqlcipher

```go
import (
	driverHook "github.com/youthlin/driver"
	sqlite3 "github.com/youthlin/go-sqlcipher"
	"github.com/youthlin/sqlcipher"
)

  // ...

  const driverName = "sqlite3_hook"

  // hook driver
	driverHook.Register(driverName, &sqlite3.SQLiteDriver{
		OnOpenHook: sqlite3.SimpleOpenHook, // for _pragma_xxx=yyy
	}, driverHook.NewHook(
		func(ctx context.Context, method driverHook.Method, query string, args any) context.Context {
      // before sql execute
			return ctx
		},
		func(ctx context.Context, method driverHook.Method, query string, args, result any, err error) (any, error) {
      // after sql execute
			return result, err
		},
	))

  // build dsn with _pragma_xxx=yyy
	name := "xxx.db"
	key := "xxxXxxx"
	params := []string{
		fmt.Sprintf(`_pragma_key=%q`, key),
		// 注意顺序 _pragma_key 最先；注意引号
		`_pragma_cipher_page_size=1024`,
		`_pragma_kdf_iter=4000`,
		`_pragma_cipher_hmac_algorithm="HMAC_SHA1"`,
		`_pragma_cipher_kdf_algorithm="PBKDF2_HMAC_SHA1"`,
		`_pragma_cipher_use_hmac="OFF"`,
	}
	dsn = fmt.Sprintf("%s?%s", name, strings.Join(params, "&"))

  // open db
	db, err := gorm.Open(&sqlcipher.Dialector{DriverName: driverName, DSN: dsn}, &gorm.Config{})

```
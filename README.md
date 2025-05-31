# go-params
Basic golang url, env, ... params parser

### Add dependency to your project  
```shell
go get github.com/rsgcata/go-params
```
  
### Usage for env parser  
To get env value as int  
```go
package main

import "github.com/rsgcata/go-params/params"

// if ENV_VAR_NAME is missing or has a string value which can't be transformed to int, envVal will
// have the value of defaultVal and defaultUsed will be true
envVal, defaultUsed := params.GetEnvAsInt("ENV_VAR_NAME", 123)
```

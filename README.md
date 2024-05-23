[![Build Status](https://travis-ci.org/elliotchance/phpserialize.svg?branch=master)](https://travis-ci.org/elliotchance/phpserialize)

PHP [serialize()](http://php.net/manual/en/function.serialize.php) and
[unserialize()](http://php.net/manual/en/function.unserialize.php) for Go.

Implementaion for https://www.phpinternalsbook.com/php5/classes_objects/serialization.html

Fork From [elliotchance/phpserialize](https://github.com/elliotchance/phpserialize)

# Install / Update

```bash
go get -u github.com/shengdoushi/phpserialize
```

# Example

```go
package main

import (
	"fmt"
	"github.com/elliotchance/phpserialize"
)

func main() {
	out, err := phpserialize.Marshal(3.2, nil)
	if err != nil {
		panic(err)
	}

	fmt.Println(string(out))

	var in float64
	err = phpserialize.Unmarshal(out, &in)

	fmt.Println(in)
}
```

### Using struct field tags for marshalling

```go
package main

import (
	"fmt"
	"github.com/elliotchance/phpserialize"
)

type MyStruct struct {
	// Will be marhsalled as my_purpose
	MyPurpose string `php:"my_purpose"`
	// Will be marshalled as my_motto, and only if not a nil pointer
	MyMotto *string `php:"my_motto,omitnilptr"`
	// Will not be marshalled
	MySecret string `php:"-"`
	// private  
	PrivateVar string `php:"privateVar,private"`
	// protected
	ProtectedVar string `php:"protectedVar,protected"`
}

func main() {
	my := MyStruct{
		MyPurpose: "No purpose",
		MySecret:  "Has a purpose",
	}

	out, err := phpserialize.Marshal(my, nil)
	if err != nil {
		panic(err)
	}

	fmt.Println(out)
}
```

# go-questdb-client

Golang client for QuestDB InfluxDB Line Protocol over TCP

**Work in progress**

```go
package main

import (
	"context"
	"fmt"
	"log"
	"time"

	questdb "github.com/questdb/go-questdb-client"
)

func main() {
	ctx := context.TODO()
	// Connect to QuestDB running on 127.0.0.1:9009
	sender, err := questdb.NewLineSender(ctx)
	if err != nil {
		log.Fatal(err)
	}
	// Make sure to close the sender on exit to release resources
	defer sender.Close()
	// Send a few ILP messages
	err = sender
		.Table("trades")
		.Symbol("name", "test_ilp1")
		.DoubleField("value", 12.4)
		.AtNow(ctx)
	if err != nil {
		log.Fatal(err)
	}
	err = sender
		.Table("trades")
		.Symbol("name", "test_ilp2")
		.DoubleField("value", 11.4)
		.At(ctx, time.Now().UnixNano())
	if err != nil {
		log.Fatal(err)
	}
	err = sender.flush(ctx)
	if err != nil {
		log.Fatal(err)
	}
}
```

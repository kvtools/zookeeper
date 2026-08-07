# Valkeyrie Zookeeper

[![GoDoc](https://godoc.org/github.com/kvtools/zookeeper?status.png)](https://godoc.org/github.com/kvtools/zookeeper)
[![Build Status](https://github.com/kvtools/zookeeper/actions/workflows/build.yml/badge.svg)](https://github.com/kvtools/zookeeper/actions/workflows/build.yml)

[`valkeyrie`](https://github.com/kvtools/valkeyrie) provides a Go native library to store metadata using Distributed Key/Value stores (or common databases).

## Compatibility

A **storage backend** in `valkeyrie` implements (fully or partially) the [Store](https://github.com/kvtools/valkeyrie/blob/main/store/store.go#L12) interface.

| Calls                 | Zookeeper |
|-----------------------|:---------:|
| Put                   |    🟢️    |
| Get                   |    🟢️    |
| Delete                |    🟢️    |
| Exists                |    🟢️    |
| Watch                 |    🟢️    |
| WatchTree             |    🟢️    |
| NewLock (Lock/Unlock) |    🟢️    |
| List                  |    🟢️    |
| DeleteTree            |    🟢️    |
| AtomicPut             |    🟢️    |
| AtomicDelete          |    🟢️    |

## Supported Versions

Zookeeper versions >= `3.4.5`.

## Examples

```go
package main

import (
	"context"
	"log"

	"github.com/kvtools/valkeyrie"
	"github.com/kvtools/zookeeper"
)

func main() {
	ctx := context.Background()

	config := &zookeeper.Config{
        Username: "example",
	}

	kv, err := valkeyrie.NewStore(ctx, zookeeper.StoreName, []string{ "localhost:8500"}, config)
	if err != nil {
		log.Fatal("Cannot create store")
	}

	key := "foo"

	err = kv.Put(ctx, key, []byte("bar"), nil)
	if err != nil {
		log.Fatalf("Error trying to put value at key: %v", key)
	}

	pair, err := kv.Get(ctx, key, nil)
	if err != nil {
		log.Fatalf("Error trying accessing value at key: %v", key)
	}

	log.Printf("value: %s", string(pair.Value))

	err = kv.Delete(ctx, key)
	if err != nil {
		log.Fatalf("Error trying to delete key %v", key)
	}
}
```

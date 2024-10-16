# Sanity client in Go

> **Under development!** For developers _with an adventurous spirit only_.

This is a client for [Sanity](https://www.sanity.io) written in Go.

## Using

See the [API reference](https://godoc.org/github.com/labordude/sanity-go) for
the full documentation.

```go
package main

import (
	"context"
	"log"

	sanity "github.com/labordude/sanity-go"
)

func main() {
	client, err := sanity.VersionV20210325.NewClient("zx3vzmn!", sanity.DefaultDataset,
		sanity.WithCallbacks(sanity.Callbacks{
			OnQueryResult: func(result *sanity.QueryResult) {
				log.Printf("Sanity queried in %d ms!", result.Time.Milliseconds())
			},
		}),
		sanity.WithToken("mytoken"))
	if err != nil {
		log.Fatal(err)
	}

	var project struct {
		ID    string `json:"_id"`
		Title string
	}
	result, err := client.
		Query("*[_type == 'project' && _id == $id][0]").
		Param("id", "123").
		Do(context.Background())
	if err != nil {
		log.Fatal(err)
	}

	if err := result.Unmarshal(&project); err != nil {
		log.Fatal(err)
    }

	log.Printf("Project: %+v", project)
}
```

## Installation

```
go get github.com/labordude/sanity-go
```

## Requirements

Go 1.13 or later.

# License

See [`LICENSE`](https://github.com/labordude/sanity-go/blob/master/LICENSE)
file.

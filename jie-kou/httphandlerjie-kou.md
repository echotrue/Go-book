```
package main

import (
	"fmt"
	"net/http"
	"log"
	"strconv"
)

type dollars float32

func (d dollars) String() string { return fmt.Sprintf("$%.2f", d) }

type database map[string]dollars

func (db database) ServeHTTP(w http.ResponseWriter, req *http.Request) {
	switch req.URL.Path {
	case "/list":
		for k, v := range db {
			fmt.Fprintf(w, "%s:%s\n", k, v)
		}
	case "/item":
		params := req.URL.Query().Get("k")
		price, ok := db[params]
		if !ok {
			w.WriteHeader(http.StatusNotFound)
			fmt.Fprintf(w, "no such item : %q\n", params)
			return
		}
		fmt.Fprintf(w, "%s\n", price)
	case "/update":
		item := req.URL.Query().Get("k")
		price := req.URL.Query().Get("p")
		if _, ok := db[item]; !ok {
			w.WriteHeader(http.StatusNotFound)
			fmt.Fprintf(w, "no such item : %q\n", item)
			return
		}
		f, err := strconv.ParseFloat(price, 64)
		if err != nil {
			w.WriteHeader(http.StatusNotFound)
			fmt.Fprintf(w, "no such item : %q\n", item)
			return
		}
		db[item] = dollars(f)
		fmt.Fprintf(w, "%s\n", db[item])

	case "/add":

	default:
		w.WriteHeader(http.StatusNotFound)
		fmt.Fprintf(w, "no such page : %q\n", req.URL)
	}

	fmt.Fprintln(w, "The goods list :")
	for k, v := range db {
		fmt.Fprintf(w, "%s's price is:%s\n", k, v)
	}
}

func main() {
	db := database{"shoes": 50, "socks": 5}
	log.Fatal(http.ListenAndServe("localhost:8000", db))
}

```




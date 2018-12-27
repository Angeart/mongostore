mongostore
==========

## This repository is minor changed from [kidstuff/mongostore](https://github.com/kidstuff/mongostore)

* Usable in [gopkg.in/mgo.v2](https://gopkg.in/mgo.v2)

[Gorilla's Session](http://www.gorillatoolkit.org/pkg/sessions) store implementation with MongoDB

## Requirements

Depends on the [mgo](https://labix.org/v2/mgo) library.

## Installation

    go get github.com/kidstuff/mongostore

## Documentation

Available on [godoc.org](http://www.godoc.org/github.com/kidstuff/mongostore).

### Example
```go
    func foo(rw http.ResponseWriter, req *http.Request) {
        // Fetch new store.
        dbsess, err := mgo.Dial("localhost")
        if err != nil {
            panic(err)
        }
        defer dbsess.Close()

        store := mongostore.NewMongoStore(dbsess.DB("test").C("test_session"), 3600, true,
            []byte("secret-key"))

        // Get a session.
        session, err := store.Get(req, "session-key")
        if err != nil {
            log.Println(err.Error())
        }

        // Add a value.
        session.Values["foo"] = "bar"

        // Save.
        if err = sessions.Save(req, rw); err != nil {
            log.Printf("Error saving session: %v", err)
        }

        fmt.Fprintln(rw, "ok")
    }
```

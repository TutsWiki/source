---
date: 2020-07-18
linktitle: A REST API in Clojure
title: A REST API in Clojure
weight: 10
url: /rest-api-in-clojure
description: A newbie friendly introduction to REST API in Clojure.
keywords:
  - clojure
  - restapi
  - programming
---

<meta property="og:image" content="https://tutswiki.com/img/tutswiki-logo.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="A REST API in Clojure" />
<meta name=”twitter:description” content="A newbie friendly introduction to REST API in Clojure." />

Clojure is one of the most interesting new languages targeting the JVM. Initially only the JVM, in the meantime it is also available for JavaScript. Essentially, you can write Clojure and either execute it as Java program or JavaScript program, of course each flavor has its unique features as well.

Clojure is a Lisp, thus the syntax may be foreign, but it is really, really easy since there are very few syntactic variations. The language “Lisp” is very lean and usually easily learned.

In this post, we’re going to create a complete REST application from scratch. There are already some (very) good tutorials available, but some are not quite up to date (see [Heroku’s Devcenter](https://devcenter.heroku.com/articles/clojure-web-application) or Mark McGranaghan for good ones). Clojure itself is still a young language, Lisp of course has a lot of history.

Our application should allow creating, listing, fetching, updating, and deleting of documents.

A document looks like this (JSON encoded):

```json
{
      "id" : "some id",
      "title" : "some title"
      "text" : "some text"
}
```

- A GET call to /documents should return a list of these documents.
- A POST call to /documents with a documents as body shall create a new document, assigning a new id (ignoreing the posted one).
- A GET to /documents/[ID] should return the document with the given id, or 404 if the document does not exist.
- A PUT to /documents/[ID] should update the document with the given id and replace title and text with those from the document in the uploaded body.
- A DELETE to /documents/[ID] should delete the document with the given id and return 204 (NO CONTENT) in any case.

## Creating the project scaffolding
We’re going to use <a href="https://leiningen.org/">Leiningen</a>, the defacto build system and dependency manager for Clojure projects. Download and install it, then execute:

```bash
lein new compojure clojure-rest
```

We’re creating a new <a href="https://github.com/weavejester/compojure/wiki">Compojure</a> project called clojure-rest. Compojure is the library that maps URLs to functions in our program. Compojure (and our project) builds on Ring is the basic Server API. To start the new project run:

```bash
lein ring server
```

This starts the server on localhost:3000 and automatically restarts the server if any of the project files change. Thus, you can leave it running while we develop our application.

The `new` command generates two very important files for us:

`project.clj` is the project configuration. It states dependencies, the entry point etc. (read the whole documentation on <a href="https://leiningen.org">Leiningen.org</a>, and `src/clojure_rest/handler.clj` which contains a starting point for our application.

## Project configuration (project.clj)
```clojure
(defproject clojure-rest "0.1.0-SNAPSHOT"
      :description "FIXME: write description"
      :url "http://example.com/FIXME"
      :dependencies [[org.clojure/clojure "1.4.0"]
                     [compojure "1.1.1"]]
      :plugins [[lein-ring "0.7.3"]]
      :ring {:handler clojure-rest.handler/app}
      :profiles
      {:dev {:dependencies [[ring-mock "0.1.3"]]}})
Update the file to look like this:

(defproject clojure-rest "0.1.0-SNAPSHOT"
      :description "REST service for documents"
      :url "https://tutswiki.com"
      :dependencies [[org.clojure/clojure "1.4.0"]
                     [compojure "1.1.1"]
                     [ring/ring-json "0.1.2"]
                     [c3p0/c3p0 "0.9.1.2"]
                     [org.clojure/java.jdbc "0.2.3"]
                     [com.h2database/h2 "1.3.168"]
                     [cheshire "4.0.3"]]
      :plugins [[lein-ring "0.7.3"]]
      :ring {:handler clojure-rest.handler/app}
      :profiles
      {:dev {:dependencies [[ring-mock "0.1.3"]]}})
```

Besides the JSON parsing library <a href="https://github.com/dakrone/cheshire">Cheshire</a>, we added the <a href="https://sourceforge.net/projects/c3p0/">C3P0 Connection Pool</a>, the <a href="https://h2database.com/">H2 Database</a> JDBC driver and Clojure’s <a href="https://github.com/clojure/java.jdbc">java.jdbc</a> contrib-library.

I also updated the `:url` and `:description` fields.

## The request handler (handler.clj)

Next let’s have a look at the generated request handler `src/clojure_rest/handler.clj`:
```clojure
(ns clojure-rest.handler
      (:use compojure.core)
      (:require [compojure.handler :as handler]
                [compojure.route :as route]))

    (defroutes app-routes
      (GET "/" [] "Hello World")
      (route/not-found "Not Found"))

    (def app
      (handler/site app-routes))
```
The route `GET "/" [] "Hello World"` is responsible for our result we saw in the browser. It maps all GET requests to / without parameters to "Hello World". The (`def app (handler/site app-routes)`) part configures our application (registering the routes).

Our first step is to update the configuration. We’re going to work with JSON, so let’s include some Ring middlewares to setup response headers (wrap-json-response) and parse request bodies (`wrap-json-body`) for us. A middleware is just a wrapper around a handler, thus it can pre- and post-process the whole request/response cycle.
```clojure
(def app
      (-> (handler/api app-routes)
        (middleware/wrap-json-body)
        (middleware/wrap-json-response)))
```
We switched also from the `handler/site` template to `handler/api` which is more appropriate for REST APIs (<a href="https://weavejester.github.com/compojure/compojure.handler.html">documentation</a>).

Next let’s define the routes for our application:
```clojure
(defroutes app-routes
      (context "/documents" [] (defroutes documents-routes
        (GET  "/" [] (get-all-documents))
        (POST "/" {body :body} (create-new-document body))
        (context "/:id" [id] (defroutes document-routes
          (GET    "/" [] (get-document id))
          (PUT    "/" {body :body} (update-document id body))
          (DELETE "/" [] (delete-document id))))))
      (route/not-found "Not Found"))
```
We define GET and POST for the context "/documents, and GET, PUT, DELETE for the context ":id" on top of that. :id is a placeholder and can then be injected into our parameter vector. The POST and PUT request have a special parameter body for the parsed body (this parameter is provided by the wrap-json-body middleware. For more on routes, take a look at <a href="https://github.com/weavejester/compojure/wiki/Routes-In-Detail">Compojure’s documentation</a>.

Before we define the functions to carry out the requests, let’s fix the imports and open a pool of database connections to work with.

The namespace declaration is used to define which namespaces shall be made available by Clojure.
```clojure
(ns clojure-rest.handler
      (:import com.mchange.v2.c3p0.ComboPooledDataSource)
      (:use compojure.core)
      (:use cheshire.core)
      (:use ring.util.response)
      (:require [compojure.handler :as handler]
                [ring.middleware.json :as middleware]
                [clojure.java.jdbc :as sql]
                [compojure.route :as route]))
```
We import C3P0’s `ComboPooledDataSource`, a plain Java class. Next, we fetch the functions defined in `compojure.core`, `cheshire.core`, and `ring.util.response` into our namespace, they can be used without qualifying. Finally we require some more libraries, this time with a qualifier to prevent name clashes or to support nicer separation. I’m not sure when to make the cut between `:use` and `:require` yet, so the cut is abitrary.
```clojure
(def db-config
      {:classname "org.h2.Driver"
       :subprotocol "h2"
       :subname "mem:documents"
       :user ""
       :password ""})
```	   
Note, we use a in-memory database. If you’d like to keep your database between restarts, you could use :subname "/tmp/documents" for example.

Next we open a pool of connections. C3P0 has no Clojure wrapper, so we deal with Java classes and objects directly (hence a bit more code).
```clojure
(defn pool
      [config]
      (let [cpds (doto (ComboPooledDataSource.)
                   (.setDriverClass (:classname config))
                   (.setJdbcUrl (str "jdbc:" (:subprotocol config) ":" (:subname config)))
                   (.setUser (:user config))
                   (.setPassword (:password config))
                   (.setMaxPoolSize 6)
                   (.setMinPoolSize 1)
                   (.setInitialPoolSize 1))]
        {:datasource cpds}))

    (def pooled-db (delay (pool db-config)))

    (defn db-connection [] @pooled-db)
```	
Since we deal with a in-memory database, we need to create our table now.
```clojure
(sql/with-connection (db-connection)
      (sql/create-table :documents [:id "varchar(256)" "primary key"]
                                   [:title "varchar(1024)"]
                                   [:text :varchar]))
```
The intent should be easy to understand, for the details take a look at the <a href="https://clojure.github.com/java.jdbc/">java.jdbc documentation</a>. We create a table documents with a :id, :title, and :text column. Note that the database column is called id, not :id.

The only thing missing are the functions to actually perform the actions requested by our clients.

To return a single document with a given id, we could come up with this:
```clojure
(defn get-document [id]
      (sql/with-connection (db-connection)
        (sql/with-query-results results
          ["select * from documents where id = ?" id]
          (cond
            (empty? results) {:status 404}
            :else (response (first results))))))
```
It reads like this: when called with an id, open a database connection, perform `select * from documents where id = ?` with the given id as parameter. If the result is empty, return 404, otherwise return the first (and only) document as response.

The response call will convert the document into JSON, this functionality is provided by wrap-json-response, which also sets the correct Content-Type etc.

Another nice one is the creation of new documents:
```clojure
(defn uuid [] (str (java.util.UUID/randomUUID)))

    (defn create-new-document [doc]
      (let [id (uuid)]
        (sql/with-connection (db-connection)
          (let [document (assoc doc "id" id)]
            (sql/insert-record :documents document)))
        (get-document id)))
```
Here we use Java’s UUID generator (without import, hence the full package name) to generate a new id for each document created. The second let statement is responsible to replace the user-provided id (if any) with our generated one. Remember that Clojure’s datastructures are immutable, so we need to use the document variable thereafter, instead of the doc which still contains the old (or no) id.

Returning the document is delegated to the get-document function.

## The complete handler.clj
To round the post up, here is the whole program:
```clojure
(ns clojure-rest.handler
      (:import com.mchange.v2.c3p0.ComboPooledDataSource)
      (:use compojure.core)
      (:use cheshire.core)
      (:use ring.util.response)
      (:require [compojure.handler :as handler]
                [ring.middleware.json :as middleware]
                [clojure.java.jdbc :as sql]
                [compojure.route :as route]))

    (def db-config
      {:classname "org.h2.Driver"
       :subprotocol "h2"
       :subname "mem:documents"
       :user ""
       :password ""})

    (defn pool
      [config]
      (let [cpds (doto (ComboPooledDataSource.)
                   (.setDriverClass (:classname config))
                   (.setJdbcUrl (str "jdbc:" (:subprotocol config) ":" (:subname config)))
                   (.setUser (:user config))
                   (.setPassword (:password config))
                   (.setMaxPoolSize 1)
                   (.setMinPoolSize 1)
                   (.setInitialPoolSize 1))]
        {:datasource cpds}))

    (def pooled-db (delay (pool db-config)))

    (defn db-connection [] @pooled-db)

    (sql/with-connection (db-connection)
    ;  (sql/drop-table :documents) ; no need to do that for in-memory databases
      (sql/create-table :documents [:id "varchar(256)" "primary key"]
                                   [:title "varchar(1024)"]
                                   [:text :varchar]))

    (defn uuid [] (str (java.util.UUID/randomUUID)))

    (defn get-all-documents []
      (response
        (sql/with-connection (db-connection)
          (sql/with-query-results results
            ["select * from documents"]
            (into [] results)))))

    (defn get-document [id]
      (sql/with-connection (db-connection)
        (sql/with-query-results results
          ["select * from documents where id = ?" id]
          (cond
            (empty? results) {:status 404}
            :else (response (first results))))))

    (defn create-new-document [doc]
      (let [id (uuid)]
        (sql/with-connection (db-connection)
          (let [document (assoc doc "id" id)]
            (sql/insert-record :documents document)))
        (get-document id)))

    (defn update-document [id doc]
        (sql/with-connection (db-connection)
          (let [document (assoc doc "id" id)]
            (sql/update-values :documents ["id=?" id] document)))
        (get-document id))

    (defn delete-document [id]
      (sql/with-connection (db-connection)
        (sql/delete-rows :documents ["id=?" id]))
      {:status 204})

    (defroutes app-routes
      (context "/documents" [] (defroutes documents-routes
        (GET  "/" [] (get-all-documents))
        (POST "/" {body :body} (create-new-document body))
        (context "/:id" [id] (defroutes document-routes
          (GET    "/" [] (get-document id))
          (PUT    "/" {body :body} (update-document id body))
          (DELETE "/" [] (delete-document id))))))
      (route/not-found "Not Found"))

    (def app
        (-> (handler/api app-routes)
            (middleware/wrap-json-body)
            (middleware/wrap-json-response)))
```
Yeah, the whole program with connection pooling, JSON de/encoding in roughly 90 lines of (admittedly dense) code.

To sum it up: Clojure is fun, concise, and very powerful. Together with the excellent Java integration it ranks very high on my “languages I adore” list.

## Help improve this content
Feel free to <div id="top-github-link">
                    <a class="github-link" title='{{T "Edit-this-page"}}' href="{{ $Site.Params.editURL }}{{ replace $File.Dir "\\" "/" }}{{ $File.LogicalName }}" target="blank">
                      <i class="fa fa-code-fork"></i>
                      <span id="top-github-link-text">{{T "Edit-this-page"}}</span>
                    </a>
                  </div> to fix any typos or add more insights by
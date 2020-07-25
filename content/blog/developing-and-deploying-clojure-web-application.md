---
date: 2020-07-25
linktitle: Developing a Simple Clojure Web Application and Deploying to AWS
title: Developing a Simple Clojure Web Application and Deploying to AWS
weight: 10
url: /developing-and-deploying-clojure-web-application-aws
description: The process of developing and deploying a simple web application in Clojure. After reading this you should be able to build your own app and deploy it to a production server.
keywords:
  - clojure
  - deploy
  - develop
  - programming
  - webapp
  - aws
  - ec2
---
<meta property="og:image" content="https://tutswiki.com/img/tutswiki-logo.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="Developing and Deploying a Simple Clojure Web Application" />
<meta name=”twitter:description” content="The process of developing and deploying a simple web application in Clojure. After reading this you should be able to build your own app and deploy it to a production server." />
The post walks through the process of developing and deploying a simple web application in Clojure. After reading this you should be able to build your own app and deploy it to a production server.

Our sample app performs addition for the user. The user enters a value in each of two text fields, the values are submitted to the app, and the app returns the corresponding sum. Eventually it will look like this:

![adder webapp](/images/adder.png "adder webapp")
![adder webapp](/images/adder2.png "adder webapp result")

Before beginning on the app, make sure that you have [Leiningen](https://leiningen.org/) installed.

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-9878675755379402"
     data-ad-slot="5842766387"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

We'll start with a minimal first version of the app. In a new directory `adder`, create a file `project.clj` with the following contents:

```clojure
(defproject adder "0.0.1"
  :description "Add two numbers."
  :dependencies
    [[org.clojure/clojure "1.2.0-beta1"]
     [org.clojure/clojure-contrib "1.2.0-beta1"]
     [ring/ring-core "0.2.5"]
     [ring/ring-devel "0.2.5"]
     [ring/ring-jetty-adapter "0.2.5"]
     [compojure "0.4.0"]
     [hiccup "0.2.6"]]
  :dev-dependencies
    [[lein-run "1.0.0-SNAPSHOT"]])
```

We'll put the main app logic in the namespace `adder.core`. Create a file at `src/adder/core.clj` with this code:

```clojure
(ns adder.core
  (:use compojure.core)
  (:use hiccup.core)
  (:use hiccup.page-helpers))

(defn view-layout [& content]
  (html
    (doctype :xhtml-strict)
    (xhtml-tag "en"
      [:head
        [:meta {:http-equiv "Content-type"
                :content "text/html; charset=utf-8"}]
        [:title "adder"]]
      [:body content])))

(defn view-input []
  (view-layout
    [:h2 "add two numbers"]
    [:form {:method "post" :action "/"}
      [:input.math {:type "text" :name "a"}] [:span.math " + "]
      [:input.math {:type "text" :name "b"}] [:br]
      [:input.action {:type "submit" :value "add"}]]))

(defn view-output [a b sum]
  (view-layout
    [:h2 "two numbers added"]
    [:p.math a " + " b " = " sum]
    [:a.action {:href "/"} "add more numbers"]))

(defn parse-input [a b]
  [(Integer/parseInt a) (Integer/parseInt b)])

(defroutes app
  (GET "/" []
    (view-input))

  (POST "/" [a b]
    (let [[a b] (parse-input a b)
          sum   (+ a b)]
      (view-output a b sum))))
```

Also, put the following in `script/run.clj`:

```clojure
(use 'ring.adapter.jetty)
(require 'adder.core)

(run-jetty #'adder.core/app {:port 8080})
```

Now you're ready to test the first version of the app:

```clojure
lein deps
lein run script/run.clj
open http://localhost:8080/
```

Check out your app in the browser. You should be to perform the simple addition described above.

As you use the app you'll probably notice changes that you'd like to make. You might also notice that errors like giving `foo` as an input are not handled well. To fix this let's apply some reloading and stacktrace middleware.

Start by including the appropriate Ring middlewares into the `adder.core` namespace definition:

```clojure
(:use ring.middleware.reload)
(:use ring.middleware.stacktrace)
```

We'll want to separate out the main app logic that we wrote earlier from the full, middleware wrapped application, so change (defroutes app to (defroutes handler and add the following at the bottom of the file:

```clojure
(def app
  (-> #'handler
    (wrap-reload '[adder.core])
    (wrap-stacktrace)))
```

After stopping your running server and restarting it with `lein run script/run.clj`, you should be able to see changes to your code in `adder.core` reflected immediately in the web interface. Also, if your application encounters any errors you will see a stacktrace indicating what went wrong:

![NumberFormatException](/images/numberformatexception.png "NumberFormatException")

Speaking of errors, we may want to address some of those in our application. If a user enters something other than a number into one of the fields, we should respond with a useful error message. Update the `view-input` function to:

```clojure
(defn view-input [& [a b]]
  (view-layout
    [:h2 "add two numbers"]
    [:form {:method "post" :action "/"}
      (if (and a b)
        [:p "those are not both numbers!"])
      [:input.math {:type "text" :name "a" :value a}] [:span.math " + "]
      [:input.math {:type "text" :name "b" :value b}] [:br]
      [:input.action {:type "submit" :value "add"}]]))
```

and update the `POST` route to:

```clojure
(POST "/" [a b]
  (try
    (let [[a b] (parse-input a b)
          sum   (+ a b)]
      (view-output a b sum))
    (catch NumberFormatException e
      (view-input a b))))
```

You can immediately verify that your changes worked by trying some invalid input:

![adder webapp](/images/adder3.png "adder webapp")

We should also handle the case where the user enters an unrecognized URL. To do that, require the Ring response middleware with:

```clojure
(:use ring.util.response)
```

and then add a catchall route to the bottom of the routes list:

```clojure
(ANY "/*" [path]
  (redirect "/"))
```

Now when you visit e.g. `/foo`, you should be redirected back to the app's main page at `/`.

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-9878675755379402"
     data-ad-slot="5842766387"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

Our app is starting to shape up, but we’re missing some necessary application infrastructure. For one, the application is not doing any logging, which makes it hard to understand what it is doing. Lets fix that with some request logging middleware. Create a new file `src/adder/middleware.clj` with these contents:

```clojure
(ns adder.middleware)

(defn- log [msg & vals]
  (let [line (apply format msg vals)]
    (locking System/out (println line))))

(defn wrap-request-logging [handler]
  (fn [{:keys [request-method uri] :as req}]
    (let [start  (System/currentTimeMillis)
          resp   (handler req)
          finish (System/currentTimeMillis)
          total  (- finish start)]
      (log "request %s %s (%dms)" request-method uri total)
      resp)))
```

Then pull this middleware into `core` with:

```clojure
(:use adder.middleware)
```

and add it to the app by updating the middleware stack to look like:

```clojure
(def app
  (-> #'handler
    (wrap-request-logging)
    (wrap-reload '[adder.middleware adder.core])
    (wrap-stacktrace)))
```

Now each request will be noted in the app's logs, along with the time it takes.

As soon as you try out the logging you'll probably notice requests to `/favicon.ico`. Since our simple app doesn't have a favicon, let's let the browser know with a 404 response. Add a `wrap-bounce-favicon` function to the `adder.middleware` namespace:

```clojure
(defn wrap-bounce-favicon [handler]
  (fn [req]
    (if (= [:get "/favicon.ico"] [(:request-method req) (:uri req)])
      {:status 404
       :headers {}
       :body ""}
      (handler req))))
```

and then include it in the middleware stack by adding (`wrap-bounce-favicon`) immediately above (`wrap-stacktrace`).

Now let's add a bit of styling to our utilitarian app. To do this we'll create and apply a CSS file that is served statically by the application. Put the following in public/adder.css:

```css
.math {
  font-family: Monaco, monospace; }

.action {
  margin-top: 2em; }
```

and update the `:head` markup in `view-layout` to look like:

```clojure
[:head
  [:meta {:http-equiv "Content-type"
          :content "text/html; charset=utf-8"}]
  [:title "adder"]
  [:link {:href "/adder.css" :rel "stylesheet" :type "text/css"}]]
```

Next, include the necessary Ring middleware:

```clojure
(:use ring.middleware.file)
(:use ring.middleware.file-info)
```

and update the middleware stack to look like:

```clojure
(def app
  (-> #'handler
    (wrap-file "public")
    (wrap-file-info)
    (wrap-request-logging)
    (wrap-reload '[adder.middleware adder.core])
    (wrap-bounce-favicon)
    (wrap-stacktrace)))
```

We should also write a few tests for our newly developed application. Create a file at `test/adder/core_test.clj` with the following contents:

```clojure
(ns adder.core-test
  (:use clojure.test)
  (:use adder.core))

(deftest parse-input-valid
  (is (= [1 2] (parse-input "1" "2"))))

(deftest parse-input-invalid
  (is (thrown? NumberFormatException
    (parse-input "foo" "bar"))))

(deftest view-output-valid
  (let [html (view-output 1 2 3)]
    (is (re-find #"two numbers added" html))))

(deftest handle-input-valid
  (let [resp (handler {:uri "/" :request-method :get})]
    (is (= 200 (:status resp)))
    (is (re-find #"add two numbers" (:body resp)))))

(deftest handle-add-valid
  (let [resp (handler {:uri "/" :request-method :post
                       :params {"a" "1" "b" "2"}})]
    (is (= 200 (:status resp)))
    (is (re-find #"1 \+ 2 = 3" (:body resp)))))

(deftest handle-add-invalid
  (let [resp (handler {:uri "/" :request-method :post
                       :params {"a" "foo" "b" "bar"}})]
    (is (= 200 (:status resp)))
    (is (re-find #"those are not both numbers" (:body resp)))))

(deftest handle-catchall
  (let [resp (handler {:uri "/foo" :request-method :get})]
    (is (= 302 (:status resp)))
    (is (= "/" (get-in resp [:headers "Location"])))))
```

You can verify that they all pass by running `lein test`.

Now that we have some tests we're ready to start thinking about deploying this app to production. We'll want the app to behave slightly differently in production and development, so we'll need a way to differentiate between the two environments. I'll use the environment variable `APP_ENV` to define `production?` and `development?` vars in the `adder.core` namespace:

```clojure
(def production?
  (= "production" (get (System/getenv) "APP_ENV")))

(def development?
  (not production?))
```

Use this var to update the middleware stack to look like:

```clojure
(def app
  (-> #'handler
    (wrap-file "public")
    (wrap-file-info)
    (wrap-request-logging)
    (wrap-if development? wrap-reload '[adder.middleware adder.core])
    (wrap-bounce-favicon)
    (wrap-exception-logging)
    (wrap-if production?  wrap-failsafe)
    (wrap-if development? wrap-stacktrace)))
```

This code will enable a public-facing failsafe middleware in production while keeping the stacktrace middleware in development. We'll also limit code reloading to development. Finally, we'll add exception logging in both cases for additional visibility. This updated stack relies on several new functions in `adder.middleware`. Add the following to the `adder.middleware` namespace declaration:

```clojure
(:require [clj-stacktrace.repl :as strp])
```

and to the `adder.middleware` body:

```clojure
(defn wrap-if [handler pred wrapper & args]
  (if pred
    (apply wrapper handler args)
    handler))

(defn wrap-exception-logging [handler]
  (fn [req]
    (try
      (handler req)
      (catch Exception e
        (log "Exception:\n%s" (strp/pst-str e))
        (throw e)))))

(defn wrap-failsafe [handler]
  (fn [req]
    (try
      (handler req)
      (catch Exception e
        {:status 500
         :headers {"Content-Type" "text/plain"}
         :body "We're sorry, something went wrong."}))))
```

The site will not run on port `8080` in production, so we'll need a way to specify the port to the run script. We'll use the `PORT` environment variable. Update the body of `script/run.clj` to the following:

```clojure
(let [port (Integer/parseInt (get (System/getenv) "PORT" "8080"))]
  (run-jetty #'adder.core/app {:port port}))
```

### Deploying to Amazon EC2
Now we’re ready to put this app into production. I'll walk through the steps needed for to deploying to EC2 using the standard [EC2 command line tools](https://aws.amazon.com/cli/), but the process would be similar for other hosting providers.

Start by setting up a security group and SSH keypair for the application:

```bash
ec2-add-group adder -d "adder deployment"
ec2-authorize adder -P tcp -p 22
ec2-authorize adder -P tcp -p 80

mkdir -p dev
ec2-add-keypair adder | tail -n +2 > dev/adder.pem
chmod 600 dev/adder.pem
```

Then allocate a server based on a public Ubunut AMI and wait for it to come up:

```bash
ec2-run-instances ami-2d4aa444 -g adder -k adder \
  -n 1 -t m1.small -z us-east-1a
watch ec2-describe-instances
```

Set some local environment variables to make subsequent commands easier:

```bash
export ADDER_PEM=dev/adder.pem
export ADDER_HOST=<ec2-public-ip>
```

To set up the server, SSH in

```bash
ssh -i $ADDER_PEM ubuntu@$ADDER_HOST
```

and run a few commands to install Java and set up the directory structure:

```bash
sudo su root
curl -L -o install-java.sh http://bit.ly/b5lesP
bash install-java.sh
mkdir -p /var/log/adder /var/adder
chown -R ubuntu:ubuntu /var/adder
```

We'll control the server process using Ubuntu's [upstart](http://upstart.ubuntu.com/). Put the following upstart configuration file in `deploy/adder.conf`:

```bash
script
  export PORT=80
  export APP_ENV=production
  cd /var/adder
  java -cp "lib/*:src/" clojure.main script/run.clj \
    >> /var/log/adder/adder.log 2>&1
end script
```

and then place it in the appropriate spot on the server with:

```bash
scp -i $ADDER_PEM deploy/adder.conf \
  ubuntu@$ADDER_HOST:/tmp/adder.conf
ssh -i $ADDER_PEM ubuntu@$ADDER_HOST \
  "sudo mv /tmp/adder.conf /etc/init/adder.conf"
```

Create a list in `deploy/exclude.txt` of files that should not be deployed to the production server:

```
.git
.gitignore
deploy
test
classes
```

Now install the app's files on the server with:

```bash
rsync --rsh='ssh -i '$ADDER_PEM \
      -vr --delete --exclude-from deploy/exclude.txt \
      ./ ubuntu@$ADDER_HOST:/var/adder/
```

After the first `rsync` completes, start the server with:

```bash
ssh -i $ADDER_PEM ubuntu@$ADDER_HOST "sudo start adder"
```

and check that it works by opening the production site from your local machine:

```bash
open http://$ADDER_HOST/
```

![adder webapp](/images/adder4.png "adder webapp result")

If you want to deploy a change, `rsync` up your code and then run:

```bash
ssh -i $ADDER_PEM ubuntu@$ADDER_HOST "sudo restart adder"
```

I hope this post helps you develop and deploy your own Clojure web applications.

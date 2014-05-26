# Devcards

Devcards helps ClojureScript developers surface graphical artifacts
quickly so that they can experience a graphical interactive workflow.
Devcards are somewhat like iPython Notebook or Gorilla REPL in that
they provide version of a visual REPL. In contrast to these browser
based editing environments Devcards are interactively produced from
code that you are editing in your source files using your favorite editor.

For example this code will surface an html template that you might be
mocking up:

```clojure
(defcard two-zero-48-view 
  (sab-card [:div.board 
              [:div.cells
                 [:div {:class "cell xpos-1 ypos-1"} 4]
                 [:div {:class "cell xpos-1 ypos-2"} 2]
                 [:div {:class "cell xpos-1 ypos-3"} 8]]]))
```

When used with lein figwheel, saving the file that contains this
definition will cause this sablono template rendered in the devcards
interface.

## Super Quick Start

There is a Leinigen template to get you up an running quickly.

Make sure you have the [latest version of leinigen installed](https://github.com/technomancy/leiningen#installation).

Type the following to create a fresh project with devcards setup for you:

```
lein new devcards hello-world
```

Then run

```
lein figwheel
```

to start the figwheel interactive devserver.

Then visit `http://localhost:3449/devcards/index.html`

## Quick Trial

If you want to see and interact with a bunch of devcards demos:

```
git clone https://github.com/bhauman/devcards.git

cd devcards

lein figwheel
```

Then visit `http://localhost:3449/devcards/index.html`

The code for cards you see in the devcards interface is located in the
`example_src` directory.

## Usage

First make sure you include the following `:dependencies` in your `project.clj` file.

```clojure
[org.clojure/clojurescript "0.0-2197"] ;; has to be at least 2197 or greater
[devcards "0.1.0-SNAPSHOT"]
```

lein figwheel is not required to use Devcards but ... if you want to
experience interactive coding with Devcards you will need to have
[lein-figwheel](https://github.com/bhauman/lein-figwheel) configured.
See the [lein-figwheel repo](https://github.com/bhauman/lein-figwheel)
for instructions on how to do that.

Devcards is extremely new so the patterns for using it are completely
up in the air so I am going to show you the least you need to setup to
get devcards running.

You will need an html file to host devcards, it makes sense to have a
separate file to host devcards.  I would create this `resources/public/devcards/index.html`
file.

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="//fb.me/react-0.9.0.js"></script>
    <script src="//code.jquery.com/jquery-1.11.0.js"></script>
    <link href="//netdna.bootstrapcdn.com/bootstrap/3.1.1/css/bootstrap.min.css" 
          rel="stylesheet" type="text/css">
    <!-- This showdown has been modified a tiny bit -->
    <script src="//rigsomelight.com/devcards/devcards-assets/showdown.js"></script>
    <link href="//rigsomelight.com/devcards/devcards-assets/devcards.css" 
          rel="stylesheet" type="text/css">
    <link href="//rigsomelight.com/devcards/devcards-assets/rendered_edn.css"
          rel="stylesheet" type="text/css">
  </head>
  <body>
    <div id="devcards-main">
    </div>
    <script src="js/compiled/out/goog/base.js" type="text/javascript"></script>
    <script src="js/compiled/example.js" type="text/javascript"></script>
    <script type="text/javascript">goog.require("example.core");</script>
  </body>
</html>
```

The key things here are to pull in the devcards requirements, provide
an element with a `devcards-main` and require your compiled
ClojureScript. If you are using `figwheel` make sure you are requiring
an `:optimizations :none` build.

Next you will need to include the Devcards library in your
ClojureScript source file. 

```
(ns example.core
  (:require
   [devcards.core :as dc :include-macros true])
  (:require-macros
   [devcards.core :refer [defcard])))

(dc/start-devcards-ui!)

;; optional
(dc/start-figwheel-reloader!)

;; required ;)
(defcard my-first-card
  (dc/sab-card [:h1 "Devcards is freaking awesome!"]))
```

## The predefined cards

### devcards.core/markdown-card

(defcard markdown-example
  (dc/markdown-card 
    "### This is markdown yo
    
    This markdown is filtered so that it can be arbitrarily indented
    
    code blocks have to be delimited like so:
    ```
    (defn myfunc [] "this is code")
    ```
    "))









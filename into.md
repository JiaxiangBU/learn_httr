
<https://httr.r-lib.org/articles/quickstart.html>

``` r
library(httr)
r <- GET("http://httpbin.org/get")
```

``` r
r
```

    ## Response [http://httpbin.org/get]
    ##   Date: 2018-12-22 02:02
    ##   Status: 200
    ##   Content-Type: application/json
    ##   Size: 325 B
    ## {
    ##   "args": {}, 
    ##   "headers": {
    ##     "Accept": "application/json, text/xml, application/xml, */*", 
    ##     "Accept-Encoding": "gzip, deflate", 
    ##     "Connection": "close", 
    ##     "Host": "httpbin.org", 
    ##     "User-Agent": "libcurl/7.54.0 r-curl/3.2 httr/1.3.1"
    ##   }, 
    ##   "origin": "117.136.8.245", 
    ## ...

会告诉什多大`Size: 325 B`

``` r
headers(r)
```

    ## $connection
    ## [1] "keep-alive"
    ## 
    ## $server
    ## [1] "gunicorn/19.9.0"
    ## 
    ## $date
    ## [1] "Sat, 22 Dec 2018 02:02:13 GMT"
    ## 
    ## $`content-type`
    ## [1] "application/json"
    ## 
    ## $`content-length`
    ## [1] "325"
    ## 
    ## $`access-control-allow-origin`
    ## [1] "*"
    ## 
    ## $`access-control-allow-credentials`
    ## [1] "true"
    ## 
    ## $via
    ## [1] "1.1 vegur"
    ## 
    ## attr(,"class")
    ## [1] "insensitive" "list"

`GET()` is used by your browser when requesting a page, and `POST()` is
(usually) used when submitting a form to a server. `PUT()`, `PATCH()`
and `DELETE()` are used most often by web APIs.

# status

> The data sent back from the server consists of three parts: the status
> line, the headers and the body.
> 
> The status code is a three digit number that summarises whether or not
> the request was successful (as defined by the server that you’re
> talking to).
> 
> A successful request always returns a status of 200.
> 
> Common errors are 404 (file not found) and 403 (permission denied).

``` r
http_status(r)
```

    ## $category
    ## [1] "Success"
    ## 
    ## $reason
    ## [1] "OK"
    ## 
    ## $message
    ## [1] "Success: (200) OK"

``` r
status_code(r)
```

    ## [1] 200

# text

``` r
content(r, "text")
```

    ## No encoding supplied: defaulting to UTF-8.

    ## [1] "{\n  \"args\": {}, \n  \"headers\": {\n    \"Accept\": \"application/json, text/xml, application/xml, */*\", \n    \"Accept-Encoding\": \"gzip, deflate\", \n    \"Connection\": \"close\", \n    \"Host\": \"httpbin.org\", \n    \"User-Agent\": \"libcurl/7.54.0 r-curl/3.2 httr/1.3.1\"\n  }, \n  \"origin\": \"117.136.8.245\", \n  \"url\": \"http://httpbin.org/get\"\n}\n"

``` r
content(r, "text", encoding = stringi::stri_enc_detect(content(r, "raw")))
```

    ## Invalid encoding list(Encoding = c("ISO-8859-1", "ISO-8859-2", "UTF-8", "UTF-16BE", "UTF-16LE", "Shift_JIS", "GB18030", "EUC-JP", "EUC-KR", "Big5", "ISO-8859-9"), Language = c("en", "ro", "", "", "", "ja", "zh", "ja", "ko", "zh", "tr"), Confidence = c(0.42, 0.2, 0.15, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.09)): defaulting to UTF-8.

    ## [1] "{\n  \"args\": {}, \n  \"headers\": {\n    \"Accept\": \"application/json, text/xml, application/xml, */*\", \n    \"Accept-Encoding\": \"gzip, deflate\", \n    \"Connection\": \"close\", \n    \"Host\": \"httpbin.org\", \n    \"User-Agent\": \"libcurl/7.54.0 r-curl/3.2 httr/1.3.1\"\n  }, \n  \"origin\": \"117.136.8.245\", \n  \"url\": \"http://httpbin.org/get\"\n}\n"

`stringi::stri_enc_detect(content(r, "raw"))`自动查询 Encoding

List化

``` r
content(r, "parsed")
```

    ## $args
    ## named list()
    ## 
    ## $headers
    ## $headers$Accept
    ## [1] "application/json, text/xml, application/xml, */*"
    ## 
    ## $headers$`Accept-Encoding`
    ## [1] "gzip, deflate"
    ## 
    ## $headers$Connection
    ## [1] "close"
    ## 
    ## $headers$Host
    ## [1] "httpbin.org"
    ## 
    ## $headers$`User-Agent`
    ## [1] "libcurl/7.54.0 r-curl/3.2 httr/1.3.1"
    ## 
    ## 
    ## $origin
    ## [1] "117.136.8.245"
    ## 
    ## $url
    ## [1] "http://httpbin.org/get"

``` r
library(tidyverse)
```

    ## ─ Attaching packages ───────────────────────── tidyverse 1.2.1 ─

    ## ✔ ggplot2 3.1.0     ✔ purrr   0.2.5
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.8
    ## ✔ tidyr   0.8.2     ✔ stringr 1.3.1
    ## ✔ readr   1.1.1     ✔ forcats 0.3.0

    ## ─ Conflicts ────────────────────────── tidyverse_conflicts() ─
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
content(r, "parsed") %>% 
    str()
```

    ## List of 4
    ##  $ args   : Named list()
    ##  $ headers:List of 5
    ##   ..$ Accept         : chr "application/json, text/xml, application/xml, */*"
    ##   ..$ Accept-Encoding: chr "gzip, deflate"
    ##   ..$ Connection     : chr "close"
    ##   ..$ Host           : chr "httpbin.org"
    ##   ..$ User-Agent     : chr "libcurl/7.54.0 r-curl/3.2 httr/1.3.1"
    ##  $ origin : chr "117.136.8.245"
    ##  $ url    : chr "http://httpbin.org/get"

# headers

``` r
headers(r)
```

    ## $connection
    ## [1] "keep-alive"
    ## 
    ## $server
    ## [1] "gunicorn/19.9.0"
    ## 
    ## $date
    ## [1] "Sat, 22 Dec 2018 02:02:13 GMT"
    ## 
    ## $`content-type`
    ## [1] "application/json"
    ## 
    ## $`content-length`
    ## [1] "325"
    ## 
    ## $`access-control-allow-origin`
    ## [1] "*"
    ## 
    ## $`access-control-allow-credentials`
    ## [1] "true"
    ## 
    ## $via
    ## [1] "1.1 vegur"
    ## 
    ## attr(,"class")
    ## [1] "insensitive" "list"

> http headers are case insensitive

``` r
headers(r)$date
```

    ## [1] "Sat, 22 Dec 2018 02:02:13 GMT"

``` r
headers(r)$DATE
```

    ## [1] "Sat, 22 Dec 2018 02:02:13 GMT"

# cookies

``` r
r <- GET("http://httpbin.org/cookies/set", query = list(a = 1))
cookies(r)
```

    ##        domain  flag path secure expiration name value
    ## 1 httpbin.org FALSE    /  FALSE       <NA>    a     1

``` r
r <- GET("http://httpbin.org/cookies/set", query = list(b = 1))
cookies(r)
```

    ##        domain  flag path secure expiration name value
    ## 1 httpbin.org FALSE    /  FALSE       <NA>    a     1
    ## 2 httpbin.org FALSE    /  FALSE       <NA>    b     1

``` r
r <- GET("http://httpbin.org/cookies/set", query = list(c = 1))
cookies(r)
```

    ##        domain  flag path secure expiration name value
    ## 1 httpbin.org FALSE    /  FALSE       <NA>    a     1
    ## 2 httpbin.org FALSE    /  FALSE       <NA>    b     1
    ## 3 httpbin.org FALSE    /  FALSE       <NA>    c     1

这样产生三个参数。

the request 和 the response 的区别

类似与 responsse

``` r
r <- GET("http://httpbin.org/get", 
  query = list(key1 = "value1", key2 = "value2")
)
content(r)$args
```

    ## $key1
    ## [1] "value1"
    ## 
    ## $key2
    ## [1] "value2"

``` r
r <- GET("http://httpbin.org/get", 
  query = list(key1 = "value 1", "key 2" = "value2", key2 = NULL))
content(r)$args
```

    ## $`key 2`
    ## [1] "value2"
    ## 
    ## $key1
    ## [1] "value 1"

`NULL`的 key-value pairs 会自动去掉。

``` r
r <- GET("http://httpbin.org/get", add_headers(Name = "Hadley"))
str(content(r)$headers)
```

    ## List of 7
    ##  $ Accept         : chr "application/json, text/xml, application/xml, */*"
    ##  $ Accept-Encoding: chr "gzip, deflate"
    ##  $ Connection     : chr "close"
    ##  $ Cookie         : chr "a=1; b=1; c=1"
    ##  $ Host           : chr "httpbin.org"
    ##  $ Name           : chr "Hadley"
    ##  $ User-Agent     : chr "libcurl/7.54.0 r-curl/3.2 httr/1.3.1"

注意， `Name = "Hadley"`并不存在于`http://httpbin.org/get`的headers中。

> Cookies are simple key-value pairs like the query string, but they
> persist across multiple requests in a session (because they’re sent
> back and forth every
time).

``` r
r <- GET("http://httpbin.org/cookies", set_cookies("MeWant" = "cookies"))
content(r)$cookies
```

    ## $MeWant
    ## [1] "cookies"
    ## 
    ## $a
    ## [1] "1"
    ## 
    ## $b
    ## [1] "1"
    ## 
    ## $c
    ## [1] "1"

> Note that this response includes the `a` and `b` cookies that were
> added by the server earlier.

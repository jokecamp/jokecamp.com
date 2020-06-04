---
author: jokecamp
date: 2019-02-17
layout: post
slug: code-examples-api-http-get-json-different-languages
title: Examples of calling an API HTTP GET for JSON in different languages
desc: "A list of code examples in various languages that demonstrate how to perform an HTTP/HTTPS GET for JSON on an API in different coding languages."
categories:
- Code
tags:
- C#
- powershell
- dart
- go
- hmac
- java
- javascript
- objectiveC
- perl
- php
- python2
- python3
- rest
- ruby
- security
- sha256
- swift
- shell
- deno
---

Downloading JSON via GET from a simple API should be the 2nd tutorial right after Hello World for every language. Below is an ever-growing collection of code examples to highlight the differences in different programming languages and serve as a practical reference.

Jump to an implementation:

  * [Node.js]({{page.url}}#nodejs)
  * [Javascript]({{page.url}}#js)
  * [Deno]({{page.url}}#deno)
  * [PHP]({{page.url}}#php)
  * [Java]({{page.url}}#java)
  * [Groovy]({{page.url}}#groovy)
  * [C#]({{page.url}}#csharp)
  * [Objective C]({{page.url}}#objc)
  * [Go]({{page.url}}#go)
  * [Ruby]({{page.url}}#ruby)
  * [Python]({{page.url}}#python)
  * [Perl]({{page.url}}#perl)
  * [Dart]({{page.url}}#dart)
  * [Swift]({{page.url}}#swift)
  * [Rust]({{page.url}}#rust)
  * [Powershell]({{page.url}}#powershell)
  * [Shell]({{page.url}}#shell)

## Contribution Rules/Guidelines

* no 3rd party libraries/packages/modules allowed (unless required for HTTPS)
* must be making an HTTPS request and use endpoint: <https://swapi.co/api/people/1/>
* keep it simple and readable
* its ok to ignore errors and best practices
* when practical/terse include the code to parse JSON too

**Please [contribute](https://github.com/jokecamp/jokecamp.com/blob/master/_posts/2019-02-17-code-examples-api-http-get-json-different-languages.markdown) (pull request) if you have a better approach or a new language.**

### <a name='nodejs'></a>Node.js HTTP GET

See [https](https://nodejs.org/api/https.html#https_https_get_options_callback)

```js
var https = require('https');

var options = {
	host: 'swapi.co',
	path: '/api/people/1/',
	headers: {
		'Accept': 'application/json'
	}
};
https.get(options, function (res) {
	var json = '';

	res.on('data', function (chunk) {
		json += chunk;
	});

	res.on('end', function () {
		if (res.statusCode === 200) {
			try {
				var data = JSON.parse(json);
				// data is available here:
				console.log(json);
			} catch (e) {
				console.log('Error parsing JSON!');
			}
		} else {
			console.log('Status:', res.statusCode);
		}
	});
}).on('error', function (err) {
	console.log('Error:', err);
});
```


### <a name='js'></a>Javascript HTTP GET

Below is a sync example using [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/send). From [StackOverflow: HTTP GET request in JavaScript](https://stackoverflow.com/a/4033310/215502)

```javascript
  var req = new XMLHttpRequest();
  req.open('GET', 'https://swapi.co/api/people/1/', false);
  req.send(null);
  console.log(req.responseText);
```

### <a name='deno'></a>Deno HTTP GET

[Deno](https://deno.land/) is a "simple, modern and secure runtime for JavaScript and TypeScript that uses V8 and is built in Rust."

You will need to use `--allow-net` to allow network access.

```javascript
// Name the below go.ts
// Then run with: deno run --allow-net go.ts https://jsonplaceholder.typicode.com/todos/
const url = Deno.args[0];
const res = await fetch(url);
const items = await res.json();
console.log(items);
```

### <a name='php'></a>PHP HTTP GET

[json_decode](http://php.net/manual/en/function.json-decode.php) and [file_get_contents](http://php.net/manual/en/function.file-get-contents.php). [Source](https://stackoverflow.com/a/959069/215502)

```php
$json = file_get_contents('https://swapi.co/api/people/1/');
echo $json;
$obj = json_decode($json);
```

### <a name='java'></a>Java HTTP GET

I don't know Java. Is this really the most simple way without 3rd party packages? [Source](https://stackoverflow.com/a/4308662/215502)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.Reader;
import java.net.URL;
import java.nio.charset.Charset;

import org.json.JSONException;
import org.json.JSONObject;

public class JsonReader {

  private static String readAll(Reader rd) throws IOException {
    StringBuilder sb = new StringBuilder();
    int cp;
    while ((cp = rd.read()) != -1) {
      sb.append((char) cp);
    }
    return sb.toString();
  }

  public static JSONObject readJsonFromUrl(String url) throws IOException, JSONException {
    InputStream is = new URL(url).openStream();
    try {
      BufferedReader rd = new BufferedReader(new InputStreamReader(is, Charset.forName("UTF-8")));
      String jsonText = readAll(rd);
      JSONObject json = new JSONObject(jsonText);
      return json;
    } finally {
      is.close();
    }
  }

  public static void main(String[] args) throws IOException, JSONException {
    JSONObject json = readJsonFromUrl("https://swapi.co/api/people/1/");
    System.out.println(json.toString());
    System.out.println(json.get("id"));
  }
}

```

### <a name='groovy'></a>Groovy HTTP GET

Apache Groovy is a Java-syntax-compatible object-oriented programming language for the Java platform. [Source](https://sites.google.com/a/athaydes.com/renato-athaydes/code/groovy---rest-client-without-using-libraries)

```groovy
def connection = new URL( "https://swapi.co/api/people/1/")
        .openConnection() as HttpURLConnection

connection.setRequestProperty( 'Accept', 'application/json' )

// get the response code - automatically sends the request
println connection.inputStream.text
```

### <a name='csharp'></a>C# HTTP GET

With [WebClient](https://docs.microsoft.com/en-us/dotnet/api/system.net.webclient?view=netframework-4.7.2). [Code Source](https://stackoverflow.com/a/5566981/215502)

```csharp
using (WebClient wc = new System.Net.WebClient())
{
   var json = wc.DownloadString("https://swapi.co/api/people/1/");
}
```

or an async and [HttpClient](https://docs.microsoft.com/en-us/dotnet/api/system.net.http.httpclient?view=netframework-4.7.2)

```csharp
using (var httpClient = new System.Net.Http.HttpClient())
{
    var json = await httpClient.GetStringAsync("https://swapi.co/api/people/1/");
}
```

### <a name='objc'></a>Objective C and Cocoa HTTP GET

[Source](https://stackoverflow.com/a/42669357/215502)

```objc
// making a GET request to /init
NSString *targetUrl = @"https://swapi.co/api/people/1/";
NSMutableURLRequest *request = [[NSMutableURLRequest alloc] init];
[request setHTTPMethod:@"GET"];
[request setURL:[NSURL URLWithString:targetUrl]];

[[[NSURLSession sharedSession] dataTaskWithRequest:request completionHandler:
  ^(NSData * _Nullable data,
    NSURLResponse * _Nullable response,
    NSError * _Nullable error) {

      NSString *myString = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
      NSLog(@"Data received: %@", myString);
}] resume];
```

### <a name='go'></a>Go programming language - Golang HTTP GET

[Source](https://gist.github.com/ijt/950790/fca88967337b9371bb6f7155f3304b3ccbf3946f)

```go
package main

import (
    "fmt"
    "net/http"
    "io/ioutil"
    "os"
    )

func main() {
    response, err := http.Get("https://swapi.co/api/people/1/")
    if err != nil {
        fmt.Printf("%s", err)
        os.Exit(1)
    } else {
        defer response.Body.Close()
        contents, err := ioutil.ReadAll(response.Body)
        if err != nil {
            fmt.Printf("%s", err)
            os.Exit(1)
        }
        fmt.Printf("%s\n", string(contents))
    }
}
```

### <a name='ruby'></a>Ruby HTTP GET

[Source](https://www.twilio.com/blog/2015/10/4-ways-to-parse-a-json-api-with-ruby.html)

```ruby
require 'net/http'
require 'json'

url = 'https://swapi.co/api/people/1/'
uri = URI(url)
response = Net::HTTP.get(uri)
JSON.parse(response)
```

### <a name='python'></a>Python HTTP GET

[Source](https://stackoverflow.com/a/12402180/215502)

```python
import requests
r = requests.get('https://swapi.co/api/people/1/')
r.json()
```
Tested with Python 2.7.12.

### <a name='perl'></a>Perl HTTP GET

[Source](https://stackoverflow.com/a/16690423/215502)

```perl
#!/usr/bin/perl

 use LWP::UserAgent;
 use HTTP::Request;

 my $URL = 'https://swapi.co/api/people/1/';

 my $ua = LWP::UserAgent->new(ssl_opts => { verify_hostname => 1 });
 my $header = HTTP::Request->new(GET => $URL);
 my $request = HTTP::Request->new('GET', $URL, $header);
 my $response = $ua->request($request);

 if ($response->is_success){
     print "URL:$URL\nHeaders:\n";
     print $response->headers_as_string;
 }elsif ($response->is_error){
     print "Error:$URL\n";
     print $response->error_as_HTML;
 }
```

For http (non https) it can be simply be:

```perl
#!/usr/bin/perl
use LWP::Simple;
my $content = get('http://jsonplaceholder.typicode.com/todos/1');
print $content
```

### <a name='dart'></a>Dart HTTP GET

[Dart](https://www.dartlang.org/) is "an object-oriented, class defined, garbage-collected language using a C-style syntax that transcompiles optionally into JavaScript." - [Wikipedia](https://en.wikipedia.org/wiki/Dart_(programming_language)). [Code Source](https://dev.to/graphicbeacon/quick-tip-how-to-make-http-requests-dart-56dd)

```dart
import 'dart:io';
import 'dart:convert';

void main() {
  HttpClient()
    .getUrl(Uri.parse('https://swapi.co/api/people/1/')) // produces a request object
    .then((request) => request.close()) // sends the request
    .then((response) => response.transform(Utf8Decoder()).listen(print)); // transforms and prints the response
}
```

### <a name='swift'></a>Swift HTTP GET

[Swift](https://swift.org/) is a "general-purpose, multi-paradigm, compiled programming language developed by Apple." -[Wikipedia](https://en.wikipedia.org/wiki/Swift_(programming_language)). [Code Source](https://stackoverflow.com/a/24016254/215502).

```c
let url = URL(string: "https://swapi.co/api/people/1/")!

let task = URLSession.shared.dataTask(with: url) {(data, response, error) in
    guard let data = data else { return }
    print(String(data: data, encoding: .utf8)!)
}

task.resume()
```

### <a name='rust'></a>Rust

[Rust](https://www.rust-lang.org/) is "a multi-paradigm systems programming language focused on safety, especially safe concurrency." - [Wikipedia](https://en.wikipedia.org/wiki/Rust_(programming_language)). Similar to C++.

[Rust does not expose an http client?](https://stackoverflow.com/q/43222429/215502). You have to use [hyper](https://github.com/hyperium/hyper) or [reqwest](https://github.com/seanmonstar/reqwest). Does anyone know if its possible with just Rust?



### <a name='powershell'></a>Powershell (Windows) HTTP GET

```c
Invoke-WebRequest "https://swapi.co/api/people/1/"
```

### <a name='shell'></a>Shell (Bash etc) HTTP GET

```bash
curl "https://swapi.co/api/people/1/"
# or
curl -X GET -H "Accept: application/json" "https://swapi.co/api/people/1/"
```



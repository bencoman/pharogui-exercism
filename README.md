# pharogui-exercism
Investigating the feasibility of a GUI interface with Pharo to Exercism.
(Updated for Exercism V2 website (note, the corresponding CLI tool seems to be V3, andn the API seems to have reverted to V1)

The first job is to work our what its service API looks like.
Exercism makes this easy with its commandline tool providing a very useful '--verbose' flag to show the wire operation of requests and responses. 
We'll start with the basic usage given at http://cli.exercism.io/usage/.

## configure
Get your API key YOUR_TOKEN here...
https://exercism.io/my/settings

Configure api key
```
$ exercism version
exercism version 3.0.8

$ exercism configure --token=YOUR_TOKEN
```

## download

Exercises are downloaded for a track with the 'download' command, e.g...

```
$ exercism --verbose download --track=go --exercise=hello-world
``` 
the output of which is... 
```
========================= BEGIN DumpRequest =========================
GET /v1/solutions/latest?exercise_id=hello-world&track_id=go HTTP/1.1
Host: api.exercism.io
Authorization: Bearer YOUR_TOKEN
Content-Type: application/json
User-Agent: github.com/exercism/cli v3.0.8 (windows/amd64)


========================= END DumpRequest =========================


========================= BEGIN DumpResponse =========================
HTTP/1.1 200 OK
Transfer-Encoding: chunked
Cache-Control: max-age=0, private, must-revalidate
Connection: keep-alive
Content-Type: application/json; charset=utf-8
Date: Sun, 26 Aug 2018 01:18:58 GMT
Etag: W/"1df219e0ab0e9451c6a7e865beee24d4"
Referrer-Policy: strict-origin-when-cross-origin
Server: nginx/1.10.3 (Ubuntu)
Set-Cookie: site_context=normal; domain=.exercism.io; path=/
Set-Cookie: _exercism_session=Rw9Px8WcBbOHbaSYFGeBGVvZVFHoXrgY2P3fSePB%2BBrhpNE46nzGREyYSY9nbQdq6WdlNdaDWvdlt8W5Qpm45KAc7%2BGMEux1aUTm%2BBNVmb%2B4OcpTzAjPOiTAbd%2BerIgj561MmWhe8uekK1f1v4g5KdT5Ww2Q%2FTZs2nxYfkKLrAq%2BeMPWrgDTMiwoUnWC%2FuwmsXFqpuZqRZnJ%2BZKCKjLxJ4RTRBTw3BONdwT9n91%2B32W%2F8hcBVbvXp%2F0qUYilDFZThlpG--ysKJgbvbbLaJWCUW--3uFmFaSCgL3we4YtaTumkA%3D%3D; domain=.exercism.io; path=/; HttpOnly
X-Content-Type-Options: nosniff
X-Download-Options: noopen
X-Frame-Options: SAMEORIGIN
X-Permitted-Cross-Domain-Policies: none
X-Request-Id: c0e32dbe-ed4e-427c-aedb-edb73e5cfbcf
X-Runtime: 0.102885
X-Xss-Protection: 1; mode=block


========================= END DumpResponse =========================


========================= BEGIN DumpRequest =========================
GET /v1/solutions/5965748a44ee4ffdb66adce1fe17cd1e/files/README.md HTTP/1.1
Host: api.exercism.io
Authorization: Bearer YOUR_TOKEN
Content-Type: application/json
User-Agent: github.com/exercism/cli v3.0.8 (windows/amd64)


========================= END DumpRequest =========================


========================= BEGIN DumpResponse =========================
HTTP/1.1 200 OK
Transfer-Encoding: chunked
Cache-Control: max-age=0, private, must-revalidate
Connection: keep-alive
Content-Type: text/plain; charset=utf-8
Date: Sun, 26 Aug 2018 01:19:01 GMT
Etag: W/"ae1a1e98b459fe4d3d93c946a66e8191"
Referrer-Policy: strict-origin-when-cross-origin
Server: nginx/1.10.3 (Ubuntu)
Set-Cookie: site_context=normal; domain=.exercism.io; path=/
Set-Cookie: _exercism_session=5PRkw2x%2B3rYtGrE7iePj2kPivNG2Wb8M8ePQzsAAeyadjrFnGs5UCBwu2%2Bbf13R9h2MlOefcOvK2h3BFeIH45qy4AbIcH91Rk5%2B1euzX6Gk9v49j6YAdVpOa68eFNkwE1fd75G0wdpgPZV9Awb9LDFbHy0sNXFxpWAB3lDwvwv9ZiUGW7GDP4hwLyzfhGcvv0fFZIKEqo%2BiSYFkod47QoVQz0KdA5sYkEruXIMsD4OZvpiUaUVv6TB06BVTT%2B3OWTgVOCQ%3D%3D--5t1ThDXrssPwwCv9--Tw2Z%2Ba0M9IwISttJ%2FyZ99g%3D%3D; domain=.exercism.io; path=/; HttpOnly
X-Content-Type-Options: nosniff
X-Download-Options: noopen
X-Frame-Options: SAMEORIGIN
X-Permitted-Cross-Domain-Policies: none
X-Request-Id: 14f78d42-b2b9-420a-85a2-8ca48176949a
X-Runtime: 0.086310
X-Xss-Protection: 1; mode=block


========================= END DumpResponse =========================


========================= BEGIN DumpRequest =========================
GET /v1/solutions/5965748a44ee4ffdb66adce1fe17cd1e/files/hello_test.go HTTP/1.1
Host: api.exercism.io
Authorization: Bearer YOUR_TOKEN
Content-Type: application/json
User-Agent: github.com/exercism/cli v3.0.8 (windows/amd64)


========================= END DumpRequest =========================


========================= BEGIN DumpResponse =========================
HTTP/1.1 200 OK
Transfer-Encoding: chunked
Cache-Control: max-age=0, private, must-revalidate
Connection: keep-alive
Content-Type: text/plain; charset=utf-8
Date: Sun, 26 Aug 2018 01:19:02 GMT
Etag: W/"81048abeb1cfcaed36184208bbfc335c"
Referrer-Policy: strict-origin-when-cross-origin
Server: nginx/1.10.3 (Ubuntu)
Set-Cookie: site_context=normal; domain=.exercism.io; path=/
Set-Cookie: _exercism_session=3RDgflzyN2XpyIDoOjNeCdo0%2B%2FFP1WUrHmZHy%2BZy%2Bw9OJZ2cT5n%2FDfZNqNeG%2FTAjWYNZlYH7dsXjBwXA3jfb0Sr6povyphO8VIzSHIGqThhv%2Fm5K67N%2B1CDlzb%2BlgB6M%2BUPUsySibMfK8n4dl7ch0SvRIUbfvCrP%2FHMFvn7rUKFlEJNM0FT2zopk3HS9wXnU7xtII1JKwNMCLuLTNlIkwqBPFg67rNMsMj0LlPzpNFtdQ4W%2FM7VhcuTaHQXAN6m8m7IovXQhDy4%3D--4ZYYafLXXLhkByt6--FIkEkJpfKUk4I6rapQeOTA%3D%3D; domain=.exercism.io; path=/; HttpOnly
X-Content-Type-Options: nosniff
X-Download-Options: noopen
X-Frame-Options: SAMEORIGIN
X-Permitted-Cross-Domain-Policies: none
X-Request-Id: ee75de9d-62d0-4fe1-9df0-6cb2f4b22df6
X-Runtime: 0.075053
X-Xss-Protection: 1; mode=block


========================= END DumpResponse =========================


========================= BEGIN DumpRequest =========================
GET /v1/solutions/5965748a44ee4ffdb66adce1fe17cd1e/files/hello_world.go HTTP/1.1
Host: api.exercism.io
Authorization: Bearer YOUR_TOKEN
Content-Type: application/json
User-Agent: github.com/exercism/cli v3.0.8 (windows/amd64)


========================= END DumpRequest =========================


========================= BEGIN DumpResponse =========================
HTTP/1.1 200 OK
Transfer-Encoding: chunked
Cache-Control: max-age=0, private, must-revalidate
Connection: keep-alive
Content-Type: text/plain; charset=utf-8
Date: Sun, 26 Aug 2018 01:19:02 GMT
Etag: W/"978a6c3cb23cbdd03fece068ffcddbe8"
Referrer-Policy: strict-origin-when-cross-origin
Server: nginx/1.10.3 (Ubuntu)
Set-Cookie: site_context=normal; domain=.exercism.io; path=/
Set-Cookie: _exercism_session=CWjoWFEEYrHIAn3DWVbKhYUMtNRohSgu18%2Fm9umUDT4SfHfV04OThb1uqsNgI4lE2wBeWybp8HXCT19vAhx58YQK6MppJ2YDk83Pf35vTMOBWZgpvpk3qfqR3Meka1AH1L%2BlJQBKILdf3iZ0nbVU%2FLRz1%2F3BY7SqQd9DXGzQteKYeFVMJyzVv5i%2BlWJxKhyIYJzUUovt2CVNtH0sZWocMg6LqV%2B4gU%2BEl6Vy1PLKtU%2FGteowFhQe0IS2%2FY8AlKjpt%2BTYIW6ndlvg--0CcB6sQdIPFEBg%2BI--L6iAKSb1D7tM5sj%2BFJicOQ%3D%3D; domain=.exercism.io; path=/; HttpOnly
X-Content-Type-Options: nosniff
X-Download-Options: noopen
X-Frame-Options: SAMEORIGIN
X-Permitted-Cross-Domain-Policies: none
X-Request-Id: 9eae9a62-f7ed-4147-9a65-6b42f8b4bbb7
X-Runtime: 0.077996
X-Xss-Protection: 1; mode=block


========================= END DumpResponse =========================

```
and now there is a new directory "C:\Users\...\Exercism\go\hello-world" holding four files.

It seems the V2 website has moved from a single request/response for all exercise files (see README-old-v2) 
to a separate request per file, and also the API key has moved from the query string to a header. 

## So try in Pharo Playground...
In Pharo 6, first install NeoJSON: Tools > Catalog Browser...  NeoJSON > Install stable version.  
Then in Playground...
```
apiKey:=YOUR_TOKEN.
exerciseId:='hello-world'.
trackId:='go'.
client := ZnClient new 
  http;
  host: 'api.exercism.io';
  path: '/v1/solutions/latest';
  headerAt: 'Authorization' put: 'Bearer ' , apiKey;
  queryAt: 'exercise_id' put: exerciseId;
  queryAt: 'track_id' put: trackId.
(response := client get) inspect.

==>String...
'{
  "solution": {
    "id": "5965748a44ee4ffdb66adce1fe17cd1e",
    "url": "https://exercism.io/my/solutions/5965748a44ee4ffdb66adce1fe17cd1e",
    "team": null,
    "user": {
      "handle": "bencoman",
      "is_requester": true
    },
    "exercise": {
      "id": "hello-world",
      "instructions_url": "https://exercism.io/my/solutions/5965748a44ee4ffdb66adce1fe17cd1e",
      "auto_approve": true,
      "track": {
        "id": "go",
        "language": "Go"
      }
    },
    "file_download_base_url": "https://api.exercism.io/v1/solutions/5965748a44ee4ffdb66adce1fe17cd1e/files/",
    "files": [
      "README.md",
      "hello_test.go",
      "hello_world.go"
    ],
    "iteration": null
  }
}'
```
More advanced NeoJSON can parse that directly into instance variables,
but quick and simple, parse that into a Dictionary...
```
solution := NeoJSONReader fromString: response.
solution inspect.
```



So one approach would be using NeoJSON to parse the response body to get a Tonel file as a string,
and parse that to create the package, classes and methods. Some discussion here...
http://forum.world.st/Tonel-Fileout-tt5079805.html

Actually I think here we dont need support for a package-per-tonel file.  The "slug" or "name" fields could provide the package name, with a tonel file per class - which presumably may provide better conformance to expectations of Execism web UI has for  other languages, than putting everything in a single file.

 

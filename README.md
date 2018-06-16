# pharogui-exercism
Investigating the feasibility of a GUI interface with Pharo to Exercism.

The first job is to work our what its service API looks like.
Exercism makes this easy with its commandline tool providing a very useful '--verbose' flag to show requests and responses. 
We'll start with the basic usage given at http://cli.exercism.io/usage/.

## fetch

Exercises are downloaded for a track with the 'fetch' command, 
replacing $TRACK_ID with the ID of the track you are working on, e.g. go/python/haskell:

```
$ exercism --verbose fetch $TRACK_ID
$ exercism --verbose fetch go
``` 

```
Request GET http://x.exercism.io/v2/exercises/go?key=c0dad43dc5e049319dde74842e39f004
Response StatusCode=200
Response Body
{   "problems": [{
        "id": "go/gigasecond",
        "track_id": "go",
        "language": "go",
        "slug": "gigasecond",
        "name": "Gigasecond",
        "files": {
            "README.md": "# Gigasecond\n\nCalculate the moment when someone has lived for 10^9 seconds.\n\nA gigasecond is 10^9 (1,000,000,000) seconds.\n\n## Running the tests\n\nTo run the tests run the command `go test` from within the exercise directory.\n\nIf the test suite contains benchmarks, you can run these with the `-bench`\nflag:\n\n    go test -bench .\n\nKeep in mind that each reviewer will run benchmarks on a different machine, with\ndifferent specs, so the results from these benchmark tests may vary.\n\n## Further information\n\nFor more detailed information about the Go track, including how to get help if\nyou're having trouble, please visit the exercism.io [Go language page](http://exercism.io/languages/go/about).\n\n## Source\n\nChapter 9 in Chris Pine's online Learn to Program tutorial. [http://pine.fm/LearnToProgram/?Chapter=09](http://pine.fm/LearnToProgram/?Chapter=09)\n\n## Submitting Incomplete Solutions\nIt's possible to submit an incomplete solution so you can see how others have completed the exercise.\n",
            "cases_test.go": "package gigasecond\n\n// Source: exercism/problem-specifications\n// Commit: 5506bac gigasecond: Apply new \"input\" policy\n// Problem Specifications Version: 1.1.0\n\n// Add one gigasecond to the input.\nvar addCases = []struct {\n\tdescription string\n\tin          string\n\twant        string\n}{\n\t{\n\t\t\"date only specification of time\",\n\t\t\"2011-04-25\",\n\t\t\"2043-01-01T01:46:40\",\n\t},\n\t{\n\t\t\"second test for date only specification of time\",\n\t\t\"1977-06-13\",\n\t\t\"2009-02-19T01:46:40\",\n\t},\n\t{\n\t\t\"third test for date only specification of time\",\n\t\t\"1959-07-19\",\n\t\t\"1991-03-27T01:46:40\",\n\t},\n\t{\n\t\t\"full time specified\",\n\t\t\"2015-01-24T22:00:00\",\n\t\t\"2046-10-02T23:46:40\",\n\t},\n\t{\n\t\t\"full time with day roll-over\",\n\t\t\"2015-01-24T23:59:59\",\n\t\t\"2046-10-03T01:46:39\",\n\t},\n}\n",
            "gigasecond.go": "// This is a \"stub\" file.  It's a little start on your solution.\n// It's not a complete solution though; you have to write some code.\n\n// Package gigasecond should have a package comment that summarizes what it's about.\n// https://golang.org/doc/effective_go.html#commentary\npackage gigasecond\n\n// import path for the time package from the standard library\nimport \"time\"\n\n// AddGigasecond should have a comment documenting it.\nfunc AddGigasecond(t time.Time) time.Time {\n\t// Write some code here to pass the test suite.\n\t// Then remove all the stock comments.\n\t// They're here to help you get started but they only clutter a finished solution.\n\t// If you leave them in, reviewers may protest!\n\treturn t\n}\n",
            "gigasecond_test.go": "package gigasecond\n\n// Write a function AddGigasecond that works with time.Time.\n\nimport (\n\t\"os\"\n\t\"testing\"\n\t\"time\"\n)\n\n// date formats used in test data\nconst (\n\tfmtD  = \"2006-01-02\"\n\tfmtDT = \"2006-01-02T15:04:05\"\n)\n\nfunc TestAddGigasecond(t *testing.T) {\n\tfor _, tc := range addCases {\n\t\tin := parse(tc.in, t)\n\t\twant := parse(tc.want, t)\n\t\tgot := AddGigasecond(in)\n\t\tif !got.Equal(want) {\n\t\t\tt.Fatalf(`FAIL: %s\nAddGigasecond(%s)\n   = %s\nwant %s`, tc.description, in, got, want)\n\t\t}\n\t\tt.Log(\"PASS:\", tc.description)\n\t}\n\tt.Log(\"Tested\", len(addCases), \"cases.\")\n}\n\nfunc parse(s string, t *testing.T) time.Time {\n\ttt, err := time.Parse(fmtDT, s) // try full date time format first\n\tif err != nil {\n\t\ttt, err = time.Parse(fmtD, s) // also allow just date\n\t}\n\tif err != nil {\n\t\t// can't run tests if input won't parse.  if this seems to be a\n\t\t// development or ci environment, raise an error.  if this condition\n\t\t// makes it to the solver though, ask for a bug report.\n\t\t_, statErr := os.Stat(\"example_gen.go\")\n\t\tif statErr == nil || os.Getenv(\"TRAVIS_GO_VERSION\") > \"\" {\n\t\t\tt.Fatal(err)\n\t\t} else {\n\t\t\tt.Log(err)\n\t\t\tt.Skip(\"(This is not your fault, and is unexpected.  \" +\n\t\t\t\t\"Please file an issue at https://github.com/exercism/go.)\")\n\t\t}\n\t}\n\treturn tt\n}\n\nfunc BenchmarkAddGigasecond(b *testing.B) {\n\tfor i := 0; i < b.N; i++ {\n\t\tAddGigasecond(time.Time{})\n\t}\n}\n"
        },
        "fresh": false
    }]
}
```

Now there is a new directory ".../go/gigasecond" holding the four files and contents shown above,
where "gigasecond" happens to be the first item on the list of exercises at http://exercism.io/languages/go/exercises. 

A specific exercise can be downloaded by specifying the exercise slug, for example *hello-world* or *food-chain*.

```
$ exercism --verbose fetch $TRACK_ID $EXERCISE_SLUG
$ exercism --verbose fetch go hello-world 
```

```
Request GET http://x.exercism.io/v2/exercises/go/hello-world?key=c0dad43dc5e049319dde74842e39f004
Response StatusCode=200
Response Body
{   "problems": [{
        "id": "go/hello-world",
        "track_id": "go",
        "language": "go",
        "slug": "hello-world",
        "name": "Hello World",
        "files": {
            "README.md": "# Hello World\n\nThe classical introductory exercise. Just say \"Hello, World!\".\n\n[\"Hello, World!\"](http://en.wikipedia.org/wiki/%22Hello,_world!%22_program) is\nthe traditional first program for beginning programming in a new language\nor environment.\n\nThe objectives are simple:\n\n- Write a function that returns the string \"Hello, World!\".\n- Run the test suite and make sure that it succeeds.\n- Submit your solution and check it at the website.\n\nIf everything goes well, you will be ready to fetch your first real exercise.\n\n## Running the tests\n\nTo run the tests run the command `go test` from within the exercise directory.\n\nIf the test suite contains benchmarks, you can run these with the `-bench`\nflag:\n\n    go test -bench .\n\nKeep in mind that each reviewer will run benchmarks on a different machine, with\ndifferent specs, so the results from these benchmark tests may vary.\n\n## Further information\n\nFor more detailed information about the Go track, including how to get help if\nyou're having trouble, please visit the exercism.io [Go language page](http://exercism.io/languages/go/about).\n\n## Source\n\nThis is an exercise to introduce users to using Exercism [http://en.wikipedia.org/wiki/%22Hello,_world!%22_program](http://en.wikipedia.org/wiki/%22Hello,_world!%22_program)\n\n## Submitting Incomplete Solutions\nIt's possible to submit an incomplete solution so you can see how others have completed the exercise.\n",
            "hello_test.go": "package greeting\n\nimport \"testing\"\n\n// Define a function named HelloWorld that takes no arguments,\n// and returns a string.\n// In other words, define a function with the following signature:\n// HelloWorld() string\n\nfunc TestHelloWorld(t *testing.T) {\n\texpected := \"Hello, World!\"\n\tif observed := HelloWorld(); observed != expected {\n\t\tt.Fatalf(\"HelloWorld() = %v, want %v\", observed, expected)\n\t}\n}\n\n// BenchmarkHelloWorld() is a benchmarking function. These functions follow the\n// form `func BenchmarkXxx(*testing.B)` and can be used to test the performance\n// of your implementation. They may not be present in every exercise, but when\n// they are you can run them by including the `-bench` flag with the `go test`\n// command, like so: `go test -bench .`\n//\n// You will see output similar to the following:\n//\n// BenchmarkHelloWorld   \t2000000000\t         0.46 ns/op\n//\n// This means that the loop ran 2000000000 times at a speed of 0.46 ns per loop.\n//\n// While benchmarking can be useful to compare different iterations of the same\n// exercise, keep in mind that others will run the same benchmarks on different\n// machines, with different specs, so the results from these benchmark tests may\n// vary.\nfunc BenchmarkHelloWorld(b *testing.B) {\n\tfor i := 0; i < b.N; i++ {\n\t\tHelloWorld()\n\t}\n}\n",
            "hello_world.go": "// This is a \"stub\" file.  It's a little start on your solution.\n// It's not a complete solution though; you have to write some code.\n\n// Package greeting should have a package comment that summarizes what it's about.\n// https://golang.org/doc/effective_go.html#commentary\npackage greeting\n\n// HelloWorld should have a comment documenting it.\nfunc HelloWorld() string {\n\t// Write some code here to pass the test suite.\n\t// Then remove all the stock comments.\n\t// They're here to help you get started but they only clutter a finished solution.\n\t// If you leave them in, reviewers may protest!\n\treturn \"\"\n}\n"
        },
        "fresh": false
    }]
}
```
Now there is a new directory ".../go/hello-world" holding the three files and contents shown above.

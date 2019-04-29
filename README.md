# gopls-monorepo-test-case

If you open `code .` at the root of this repo, `gopls` will fail with errors like:

```
[Error - 10:26:16 am] Request textDocument/codeAction failed.
  Message: no file information for file:///home/brad/Projects/Personal/gopls-monorepo-test-case/bar/main.go
  Code: 0 
```

This can be further demonstrated by running a command like `gopls format ./bar/main.go` and getting the results:

```
2019/04/29 10:30:00 Log:cacheAndDiagnose: file:///home/brad/Projects/Personal/gopls-monorepo-test-case/bar/main.go
2019/04/29 10:30:00 Log:cacheAndDiagnose: set content for file:///home/brad/Projects/Personal/gopls-monorepo-test-case/bar/main.go
2019/04/29 10:30:00 Log:cacheAndDiagnose: returned from diagnostics for file:///home/brad/Projects/Personal/gopls-monorepo-test-case/bar/main.go
format: /home/brad/Projects/Personal/gopls-monorepo-test-case/bar/main.go: no file information for file:///home/brad/Projects/Personal/gopls-monorepo-test-case/bar/main.go
```

Running the commands `cd ./bar && gopls format main.go && cd -` results in:

```
2019/04/29 10:31:28 Log:cacheAndDiagnose: file:///home/brad/Projects/Personal/gopls-monorepo-test-case/bar/main.go
2019/04/29 10:31:28 Log:cacheAndDiagnose: set content for file:///home/brad/Projects/Personal/gopls-monorepo-test-case/bar/main.go
2019/04/29 10:31:28 Log:cacheAndDiagnose: returned from diagnostics for file:///home/brad/Projects/Personal/gopls-monorepo-test-case/bar/main.go
package main

import "fmt"

func main() {
	fmt.Println("Hello from bar")
}
2019/04/29 10:31:29 Log:cacheAndDiagnose: going to get diagnostics for file:///home/brad/Projects/Personal/gopls-monorepo-test-case/bar/main.go
2019/04/29 10:31:29 Log:running `go vet` analyses for file:///home/brad/Projects/Personal/gopls-monorepo-test-case/bar/main.go
/home/brad/Projects/Personal/gopls-monorepo-test-case
```

I understand that it is perhaps usual to have many `go.mod` files in a single repo.

> Adding modules, removing modules, and versioning modules in such a
> configuration require considerable care and deliberation, so it is
> almost always easier and simpler to manage a single-module repository
> rather than multiple modules in an existing repository.
> _<https://github.com/golang/go/wiki/Modules#faqs--multi-module-repositories>_

However in this case I believe it makes sense, the actual repo I am working with
contains many projects, not all of them built in _golang_ and of the ones that
are they are somewhat separate anyway.

Just as changing into the individual projects directory works above for
`gopls format` running `code ./bar` works as expected.
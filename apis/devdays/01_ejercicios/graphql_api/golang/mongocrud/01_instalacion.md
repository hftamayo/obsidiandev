
1. go mod init github.com/hftamayo/gqlmongocrud

2. go get github.com/99designs/gqlgen

3. printf '// +build tools\npackage tools\nimport _ "github.com/hftamayo/gqlmongocrud"' | gofmt > tools.go

4. go mod tidy

5. go get github.com/99designs/gqlgen

![[Pasted image 20231010151701.png]]


6. go run github.com/99designs/gqlgen init

![[Pasted image 20231010151632.png]]


7. 
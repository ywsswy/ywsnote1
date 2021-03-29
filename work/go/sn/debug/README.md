go get -u github.com/derekparker/delve/cmd/dlv

dlv debug test.go
> b <package>.<function>
> c
> p <var>
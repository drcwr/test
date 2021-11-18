### install pprof and graphviz tools
go get -u github.com/google/pprof

https://graphviz.org/download/
https://gitlab.com/api/v4/projects/4207231/packages/generic/graphviz-releases/2.49.0/stable_windows_10_cmake_Release_x64_graphviz-install-2.49.0-win64.exe
system path and all users

### go test
echo "go test"
go test -bench=. -cpuprofile=cpu.prof
echo "pprof"
pprof -http=:6060 cpu.prof

### show flame graph
#http://127.0.0.1:6060/ui/flamegraph?si=cpu


demo:
[github.com/drcwr/godemos/map-struct-json-marshal-benchmark](https://github.com/drcwr/godemos/tree/master/map-struct-json-marshal-benchmark)



https://segmentfault.com/a/1190000016412013
https://www.huaweicloud.com/articles/b74fb5a951a4b3cadd28ffba08f52112.html
---
title: golang笔记
date: 2018-10-31 01:36:30
tags: [golang]
categories: web
---
## Go环境变量
### GOROOT
  golang当前的安装目录
### GOPATH
  golang工作区的集合，放置golang源码文件的目录，包含以下三个目录
  ```
    ├── bin  //编译后的可执行文件
    ├── pkg  //存放go install命令安装后的代码包的归档文件
    └── src  //源码存放，命令源码文件并不一定必须放在 src 文件夹
  ```
### GOBIN
  指向编译后的可执行文件，上述的bin目录
### GOOS 和 GOARCH
  两个环境变量是不用我们设置的，系统就默认的。GOOS 是 Go 所在的操作系统类型，GOARCH 是 Go 所在的计算架构。
  <!--more-->
## Go命令基础
### go run 
`go run`用于运行命令源码文件，只接受一个<b>命令源码文件</b>`，go run`包含了`go build`的编译过程，可以接受`go build`的所有命令参数。
```
go run [build flags] [-exec xprog] gofiles... [arguments...]
```
### go build
`go build`编译制定的源码文件或代码包及其依赖包
```
go build [-o output] [-i] [build flags] [packages]
```
参数`[-i]`:
  - `-a`对所有的相关代码包进行重新构建
  - `-n`打印编译时用到的命令，不执行
  - `-p n`指定编译时执行过程中的并行数量，默认为CPU的逻辑数量
  - `-race`开启竞态条件检测
  - `-work`打印编译时生成的临时工作目录的路径，编译结束后保留(默认删除)
  - `-x`打印编译期间用到的其他的命令，并将执行这些命令
  - `-asmflags`
  - `-buildmode` 用于制定编译的模式`-buildmode=default`(使用默认的设置)，用于控制编译完成后生成静态链接库（即.a文件，也就是我们之前说的归档文件）、动态链接库（即.so文                   。件）或/和可执行文件。
  - `gcflags`用于制定需要传递给`go tool compile`的命令的标记文件

### go install
  编译并安装制定的代码包及它们的依赖包，比`go build`多一步，将编译后的结果文件安装到指定的目录
### go get
  将从互联网下载或更行制定的代码及其依赖包，并将编译和安装
  ```
  go get [-d] [-f] [-fix] [-insecure] [-t] [-u] [build flags] [packages]
  ```
  - `-d` 命令只会执行下载操作，而不会执行安装动作
  - `-f` 与`-u`一同执行时才有效，让命令程序忽略已下载代码包的导入路径的检查。
  - `-fix` 下载代码包后先执行修正操作，在进行编译和安装。
  - `-insecure`允许使用非安全的`scheme`（如http）去下载指定的代码包。
  - `-t` 下载安装指定的代码包中的测试源码文件中的依赖的代码包
  - `-u` 利用网络更新已有代码包及其依赖包。
  - `-x` 查看`go get`执行过程中使用的所有命令

### go list
  列出指定的代码包的信息
  ```
   go list [-e] [-f format] [-json] [build flags] [packages]
  ```
### go test
  ```
   go test [build/test flags] [packages] [build/test flags & test binary flags]
  ```
  - `-c`生成用于测试的可执行文件，不执行
  - `-i`安装运行测试所需要的依赖包，不编译测试运行测试代码
  - `-o`指定用于运行测试可执行文件的名称，不影响测试代码的运行

### go tool
  ```
  go tool [-n] command [args...]
  ```
  - go tool pprof命令来交互式的访问概要文件的内容。会分析指定的概要文件，并会提供高可读性的输出信息。
  - go tool cgo这个工具可以使我们创建能够调用C语言代码的Go语言源码文件。

### go fmt
  ```
  go fmt [-n] [-x] [packages] 
  ```
  - `-n`打印编译期间所用到的其它命令，但是不执行它们。
  - `-x`可以看到`go get`命令执行过程中所使用的所有命令。

## go clean
删除执行其他命令时产生的文件和目录(go build和go test)
```
 go clean [-i] [-r] [-n] [-x] [build flags] [packages] 
```
- `- r`当前代码包的所有依赖包及`go build`和`go test`相关
- `-i`除安装当前代码包时所产生的结果文件

## go doc
可以打印附于Go语言程序实体上的文档。我们可以通过把程序实体的标识符作为该命令的参数来达到查看其文档的目的。
```
go doc [-u] [-c] [package|[package.]symbol[.method]]
```
- `-c`加入此标记后会使go doc命令区分参数中字母的大小写。
- `-cmd`打印出main包中的可导出的程序实体（其名称的首字母大写）的文档。
- `-u`打印出不可导出的程序实体（其名称的首字母小写）的文档。
- `godoc`用于展示指定代码包的文档。在Go语言的1.5版本以后，它是一个内置的标准命令。
```
godoc -http=:6060标记-http的值:6060表示启动的Web服务器使用本机的6060端口。
```
## go generate
`go generate`可以用来自动自成Go代码的命令。
# go语言特性

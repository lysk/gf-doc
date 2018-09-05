# glog

通用日志管理模块。

使用方式：
```go
import "gitee.com/johng/gf/g/os/glog"
```

方法列表： [godoc.org/github.com/johng-cn/gf/g/os/glog](http://godoc.org/github.com/johng-cn/gf/g/os/glog)
```go
func Critical(v ...interface{})
func Criticalf(format string, v ...interface{})
func Criticalfln(format string, v ...interface{})
func Debug(v ...interface{})
func Debugf(format string, v ...interface{})
func Debugfln(format string, v ...interface{})
func Error(v ...interface{})
func Errorf(format string, v ...interface{})
func Errorfln(format string, v ...interface{})
func Fatal(v ...interface{})
func Fatalf(format string, v ...interface{})
func Fatalfln(format string, v ...interface{})
func Fatalln(v ...interface{})
func GetLogPath() string
func Info(v ...interface{})
func Infof(format string, v ...interface{})
func Infofln(format string, v ...interface{})
func Notice(v ...interface{})
func Noticef(format string, v ...interface{})
func Noticefln(format string, v ...interface{})
func Panic(v ...interface{})
func Panicf(format string, v ...interface{})
func Panicfln(format string, v ...interface{})
func Panicln(v ...interface{})
func Print(v ...interface{})
func Printf(format string, v ...interface{})
func Printfln(format string, v ...interface{})
func Println(v ...interface{})
func SetDebug(debug bool)
func SetLogPath(path string)
func Warning(v ...interface{})
func Warningf(format string, v ...interface{})
func Warningfln(format string, v ...interface{})
type Logger
    func New() *Logger
    func (l *Logger) Critical(v ...interface{})
    func (l *Logger) Criticalf(format string, v ...interface{})
    func (l *Logger) Criticalfln(format string, v ...interface{})
    func (l *Logger) Debug(v ...interface{})
    func (l *Logger) Debugf(format string, v ...interface{})
    func (l *Logger) Debugfln(format string, v ...interface{})
    func (l *Logger) Error(v ...interface{})
    func (l *Logger) Errorf(format string, v ...interface{})
    func (l *Logger) Errorfln(format string, v ...interface{})
    func (l *Logger) Fatal(v ...interface{})
    func (l *Logger) Fatalf(format string, v ...interface{})
    func (l *Logger) Fatalfln(format string, v ...interface{})
    func (l *Logger) Fatalln(v ...interface{})
    func (l *Logger) GetDebug() bool
    func (l *Logger) GetLastLogDate() string
    func (l *Logger) GetLogIO() io.Writer
    func (l *Logger) GetLogPath() string
    func (l *Logger) Info(v ...interface{})
    func (l *Logger) Infof(format string, v ...interface{})
    func (l *Logger) Infofln(format string, v ...interface{})
    func (l *Logger) Notice(v ...interface{})
    func (l *Logger) Noticef(format string, v ...interface{})
    func (l *Logger) Noticefln(format string, v ...interface{})
    func (l *Logger) Panic(v ...interface{})
    func (l *Logger) Panicf(format string, v ...interface{})
    func (l *Logger) Panicfln(format string, v ...interface{})
    func (l *Logger) Panicln(v ...interface{})
    func (l *Logger) Print(v ...interface{})
    func (l *Logger) Printf(format string, v ...interface{})
    func (l *Logger) Printfln(format string, v ...interface{})
    func (l *Logger) Println(v ...interface{})
    func (l *Logger) SetDebug(debug bool)
    func (l *Logger) SetLogIO(w io.Writer)
    func (l *Logger) SetLogPath(path string) error
    func (l *Logger) Warning(v ...interface{})
    func (l *Logger) Warningf(format string, v ...interface{})
    func (l *Logger) Warningfln(format string, v ...interface{})
```
需要特别说明的几点：
1. ```Debug*```方法是调试方法，可以使用```SetDebug```开启/关闭掉输出信息；
2. 非```Print*/Info*```的方法都是错误信息日志方法，错误信息方法在打印错误信息的同时也会打印出对应的调用trace信息；
3. ```Fatal*```方法在输出错误信息之后会停止进程运行；
4. 日志内容默认是打印到标准输出和标准错误设备上，使用```SetLogPath```可以设定日志的物理化文件存储目录，日志文件固定按照日期来命名，例如：20170102.log；

## Trace特性

错误日志信息支持trace功能，来一个例子：
```go
package main

import (
    "gitee.com/johng/gf/g/os/glog"
)

func main() {
    glog.Error("发生错误！")
}
```

打印出的结果如下：
```shell
2018-01-11 17:20:10 [ERRO] 发生错误！
Trace:
1. /home/john/Workspace/Go/gitee.com/johng/gf/geg/other/test.go:8
2. /home/john/Softs/go1.9.2/src/runtime/proc.go:195
3. /home/john/Softs/go1.9.2/src/runtime/asm_amd64.s:2337
```

错误信息的trace信息对于调试错误来说相当有用。

## Debug特性

```go
package main

import (
    "time"
    "gitee.com/johng/gf/g/os/glog"
    "gitee.com/johng/gf/g/os/gtime"
)

func main() {
    gtime.SetTimeout(3*time.Second, func() {
        glog.SetDebug(false)
    })
    for {
        glog.Debug(gtime.Datetime())
        time.Sleep(time.Second)
    }
}
```
该示例中使用```glog.Debug```方法输出调试信息，3秒后关闭调试信息的输出。执行后，输出结果如下：
```html
2018-07-22 12:20:11.100 [DEBU] 2018-07-22 12:20:11
2018-07-22 12:20:12.101 [DEBU] 2018-07-22 12:20:12
2018-07-22 12:20:13.101 [DEBU] 2018-07-22 12:20:13
```
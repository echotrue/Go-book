在 Go 语言中，文件使用指向 os.File 类型的指针来表示的，也叫做文件句柄。我们在前面章节使用到过标准输入 os.Stdin 和标准输出 os.Stdout，他们的类型都是 \*os.File。让我们来看看下面这个程序：

```
func main() {
    inputFile, inputError := os.Open("inter/test.txt")
    if inputError != nil {
        fmt.Printf("An error occurred on opening the inputfile\n" +
            "Does the file exist?\n" +
            "Have you got acces to it?\n")
        return // exit the function on error
    }
    defer inputFile.Close()

    inputReader := bufio.NewReader(inputFile)
    for {
        inputString, readerError := inputReader.ReadString('\n')

        if inputString == "\n" {
            continue
        }
        fmt.Printf("The input was: %q\n", inputString)
        if readerError == io.EOF {
            return
        }
    }
}
```

##### 将整个文件的内容读到一个字符串里

如果您想这么做，可以使用 io/ioutil 包里的 ioutil.ReadFile\(\) 方法，该方法第一个返回值的类型是 \[\]byte，里面存放读取到的内容，第二个返回值是错误，如果没有错误发生，第二个返回值为 nil。请看示例 12.5。类似的，函数 WriteFile\(\) 可以将 \[\]byte 的值写入文件。


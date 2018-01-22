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

```
inputFile := "inter/test.txt"
    outputFile := "new.txt"
    buf, err := ioutil.ReadFile(inputFile)
    if err != nil {
        fmt.Fprintf(os.Stderr, "File Error: %s\n", err)
    }
    fmt.Printf("%s", buf)
    err = ioutil.WriteFile(outputFile, buf, 0644)
    if err != nil {
        panic(err.Error())
    }
```

##### 按列读取文件中的数据

文件 products.txt 的内容如下：



"The ABC of Go";25.5;1500

"Functional Programming with Go";56;280

"Go for It";45.9;356

"The Go Way";55;500

每行的第一个字段为 title，第二个字段为 price，第三个字段为 quantity。内容的格式基本与 示例 12.3c 的相同，除了分隔符改成了分号。请读取出文件的内容，创建一个结构用于存取一行的数据，然后使用结构的切片，并把数据打印出来。




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

**将整个文件的内容读到一个字符串里**


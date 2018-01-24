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

example1:

```
type Book struct {
    title    string
    price    float64
    quantity int
}

func main() {
    bks := make([]Book, 1)
    file, err := os.Open("products.txt")
    if err != nil {
        log.Fatalf("Error %s opening file products.txt: ", err)
    }
    defer file.Close()

    reader := bufio.NewReader(file)
    for {
        // read one line from the file:
        line, err := reader.ReadString('\n')
        if err == io.EOF {
            break
        }
        // remove \r and \n so 2(in Windows, in Linux only \n, so 1):
        line = string(line[:len(line)-2])
        //fmt.Printf("The input was: -%s-", line)

        strSl := strings.Split(line, ";")
        book := new(Book)
        book.title = strSl[0]
        book.price, err = strconv.ParseFloat(strSl[1], 32)
        if err != nil {
            fmt.Printf("Error in file: %v", err)
        }
        //fmt.Printf("The quan was:-%s-", strSl[2])
        book.quantity, err = strconv.Atoi(strSl[2])
        if err != nil {
            fmt.Printf("Error in file: %v", err)
        }
        if bks[0].title == "" {
            bks[0] = *book
        } else {
            bks = append(bks, *book)
        }
    }
    fmt.Println("We have read the following books from the file: ")
    for _, bk := range bks {
        fmt.Println(bk)
    }
}
```

example 2:

```
type Book struct {
    title    string
    price    float64
    quantity int
}

func main() {
    bk := make([]Book, 1)
    item, err := ioutil.ReadFile("new.txt")
    if err != nil {
        fmt.Fprintf(os.Stderr, "File Error: %s\n", err)
    }
    str := string(item)
    newStr := strings.Split(str, "\n")

    //fmt.Println(len(newStr))

    for _, strItem := range newStr {
        //fmt.Println(strItem)//"The ABC of Go";25.5;1500

        strSl := strings.Split(strItem, ";")

        //fmt.Println(strSl)
        book := new(Book)
        book.title = strSl[0]
        book.price, err = strconv.ParseFloat(strSl[1], 32)
        if err != nil {
            fmt.Printf("Error in file float: %v", err)
        }

        book.quantity, err = strconv.Atoi(strSl[2])
        if err != nil {
            fmt.Printf("%v", strSl[2])
            fmt.Printf("Error in file int: %v", err)
        }
        if bk[0].title == "" {
            bk[0] = *book
        } else {
            bk = append(bk, *book)
        }
    }

    fmt.Println("We have read the following books from the file: ")
    for _, bk := range bk {
        fmt.Println(bk)
    }
}
```

##### 写文件




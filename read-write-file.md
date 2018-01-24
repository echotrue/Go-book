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

```
    outPutFile, outPutErr := os.OpenFile("axlrose.txt", os.O_WRONLY|os.O_CREATE, 0666)
    if outPutErr != nil {
        fmt.Printf("An error occurred with file opening or creation\n")
        return
    }

    defer outPutFile.Close()

    outPutWriter := bufio.NewWriter(outPutFile)
    outPutWriter.WriteString("hello world")
    outPutWriter.Flush()
```

除了文件句柄，我们还需要`bufio`的`Writer`。我们以只写模式打开文件`output.dat`，如果文件不存在则自动创建：

```
outputFile
, 
outputError
:=
 os.
OpenFile
(“output.
dat
”, os.
O_WRONLY
|os.
O_CREATE
, 
0666
)
```

可以看到，`OpenFile`函数有三个参数：文件名、一个或多个标志（使用逻辑运算符“\|”连接），使用的文件权限。

我们通常会用到以下标志：

* `os.O_RDONLY`
  ：只读
* `os.O_WRONLY`
  ：只写
* `os.O_CREATE`
  ：创建：如果指定文件不存在，就创建该文件。
* `os.O_TRUNC`
  ：截断：如果指定文件已存在，就将该文件的长度截为0。

在读文件的时候，文件的权限是被忽略的，所以在使用`OpenFile`时传入的第三个参数可以用0。而在写文件时，不管是 Unix 还是 Windows，都需要使用 0666。

然后，我们创建一个写入器（缓冲区）对象：

```
outputWriter
:=
 bufio.
NewWriter
(outputFile)
```

接着，使用一个 for 循环，将字符串写入缓冲区，写 10 次：`outputWriter.WriteString(outputString)`

缓冲区的内容紧接着被完全写入文件：`outputWriter.Flush()`

如果写入的东西很简单，我们可以使用`fmt.Fprintf(outputFile, “Some test data.\n”)`直接将内容写入文件。`fmt`包里的 F 开头的 Print 函数可以直接写入任何`io.Writer`，包括文件（请参考[章节12.8](https://github.com/Unknwon/the-way-to-go_ZH_CN/blob/master/eBook/12.8.md)\).

##### 习题

程序中的数据结构如下，是一个包含以下字段的结构:

```
type Page struct {
    Title string
    Body  []byte
}
```

请给这个结构编写一个`save`方法，将 Title 作为文件名、Body作为文件内容，写入到文本文件中。

再编写一个`load`函数，接收的参数是字符串 title，该函数读取出与 title 对应的文本文件。请使用`*Page`做为参数，因为这个结构可能相当巨大，我们不想在内存中拷贝它。请使用`ioutil`包里的函数

```
type Page struct {
    title string
    body  []byte
}

func (this *Page) save() (err error) {
    return ioutil.WriteFile(this.title, this.body, 0666)
}

func (this *Page) load(title string) (err error) {
    this.title = title
    this.body, err = ioutil.ReadFile(this.title)
    return err
}
func main() {
    page := Page{
        "Page.md",
        []byte("# Page\n## Section1\nThis is section1."),
    }
    page.save()

    // load from Page.md
    var new_page Page
    new_page.load("Page.md")
    fmt.Println(string(new_page.body))
}
```




```
func cat(f *os.File) {
	const NBUF = 512
	var buf [NBUF]byte
	for {
		//从文件读取内容到字节切片buf
		switch nr, err := f.Read(buf[:]); true {
		case nr < 0:
			fmt.Fprintf(os.Stderr, "cat: error reading: %s\n", err.Error())
			os.Exit(1)
		case nr == 0: // EOF
			return
		case nr > 0:
			//写入文件内容到os.Stdout
			if nw, ew := os.Stdout.Write(buf[0:nr]); nw != nr {
				fmt.Fprintf(os.Stderr, "cat: error writing: %s\n", ew.Error())
			}
		}
	}
}

func main() {
	flag.Parse() //解析命令行参数

	if flag.NArg() == 0 {
		//命令行没有参数则从os.Stdin读取输入
		cat(os.Stdin)
	}
	for i := 0; i < flag.NArg(); i++ {
		//打开文件
		f, err := os.Open(flag.Arg(i))
		if f == nil {
			fmt.Fprintf(os.Stderr, "cat: can't open %s: error %s\n", flag.Arg(i), err)
			os.Exit(1)
		}
		//打印文件内容
		cat(f)
		f.Close()
	}
}
```




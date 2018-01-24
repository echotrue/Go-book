```
func copyFile(sourceFile, targetFile string) (written int64, err error) {
    from, err := os.Open(sourceFile)
    if err != nil {
        return
    }

    defer from.Close()

    toFile, err := os.OpenFile(targetFile, os.O_WRONLY|os.O_CREATE, 0666)
    if err != nil {
        return
    }

    defer toFile.Close()
    return io.Copy(toFile, from)
}
```




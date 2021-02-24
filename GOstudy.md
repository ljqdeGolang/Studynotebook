1、通道和并发

```go
go func chan int) { //读写均可的channel c } (a)
go func(c <- chan int) { //只读的Channel } (a)
go func(c chan <- int) {  //只写的Channel } (a)
```

2、接口对象执行的类型动态转换/查询——类型断言

```go
a,ok :=v.([]byte)//判断v是否保存了字节切片类型的值，如果有，就返回ok=1 

```

3、当用遍历range遍历切片时，改变其值，也能改变切片的值吗？


```go
go func(c chan int) { //读写均可的channel c } (a)
go func(c <- chan int) { //只读的Channel } (a)
go func(c chan <- int) {  //只写的Channel } (a)
```
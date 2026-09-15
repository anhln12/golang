Hướng dẫn setup môi trường lập trình Golang với Visual Studio Code

1. Cài đặt Golang Compiler

Để cài đặt Golang thì các bạn cần truy cập vào: ```https://go.dev/dl``` và chọn gói cài đặt tương thích với hệ điều hành máy tính của các bạn.

2. Lựa chọn và cài đặt text editor cho Golang

Một lựa chọn editor phổ biến đó là Visual Studio Code, hoàn toàn miễn phí và gọn nhẹ.
- Tải và cài đặt https://code.visualstudio.com/Download
- Khởi động Visual Studio Code và chuyển đến tab Extension
<img width="1085" height="988" alt="image" src="https://github.com/user-attachments/assets/4b20ae4d-da77-4656-ac9f-0ff55132a54a" />
- Tìm kiếm extension "Go" và chọn extension "Go" có dấu stick xanh "Go Team at Google". Sau đó bấm cài đặt và khởi động lại editor.
<img width="812" height="566" alt="image" src="https://github.com/user-attachments/assets/949198b2-251d-4347-a7b0-593e8a5937da" />
- Extension Go sẽ yêu cầu thêm một số tools bổ sung, các bạn hãy chọn cho phép cài đặt chúng khi có popup

3. Viết chương trình Golang đầu tiên với Visual Studio Code
- Tạo môt folder chưa source code golang: first-go-app
- Khởi động VS và Open Folder bạn tạo ở B1
- Tạo file mới trong folder đặt tên main.go
- Copy & paste code dưới vào file main.go
```
package main

import "fmt"

func main() {
	fmt.Println("Hello world")
}
```

4. Chạy tuhwr chương trình golang đầu tiên với VS
- Run code bằng lệnh run
```
go run main.go
```

- Compile code golang ra file thực thi bằng lệnh go build
```
go mod init first-go-app
go build -o app
```

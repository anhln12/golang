1. Cài đặt go trên ubuntu
```
apt install golang-go
```

2. ff
Từ Go 1.17+, lệnh go get không còn được dùng cài package ngoài module nếu bạn chưa khởi tạo go module (go.mod chưa tồn tại)

B1: Khởi tạo module Go (Bắt buộc)
```
mkdir process_monitor
cd process-monitor
go mod init process-monitor
```

B2: Cài thư viện Promethus client
```
go get github.com/prometheus/client_golang/prometheus@v1.14.0
go get github.com/prometheus/client_golang/prometheus/promhttp@v1.14.0
go run github.com/prometheus/client_golang/prometheus@latest
go run gopkg.in/yaml.v2@latest
```

B3: Kiểm tra kết quả
```
go.mode --> chứa các dependency
go.suym --> chứa hash checksum để đảm bảo an toàn
```

B4: Cấu trúc 
```
|----config.yaml
|----process_monitor.go
```

B5: nội dung file config.yaml
```
groups:
  web:
    - nginx
    - apache2
  db:
    - postgres
    - mysqld
  system:
    - sshd
```

B6: Nội dung file process_monitor.go
```
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"os/exec"
	"strings"
	"time"

	"gopkg.in/yaml.v2"

	"github.com/prometheus/client_golang/prometheus"
	"github.com/prometheus/client_golang/prometheus/promhttp"
)

// Cấu trúc YAML
type Config struct {
	Groups map[string][]string `yaml:"groups"`
}

// Prometheus metric
var processStatus = prometheus.NewGaugeVec(
	prometheus.GaugeOpts{
		Name: "linux_process_up",
		Help: "Trạng thái của process (UP=1, DOWN=0), theo group và tên.",
	},
	[]string{"group", "process_name"},
)

func init() {
	prometheus.MustRegister(processStatus)
}

// Kiểm tra process có đang chạy không
func isRunning(name string) bool {
	cmd := exec.Command("pgrep", "-x", name)
	output, _ := cmd.Output()
	return strings.TrimSpace(string(output)) != ""
}

// Đọc file YAML
func readConfig(path string) (*Config, error) {
	data, err := ioutil.ReadFile(path)
	if err != nil {
		return nil, err
	}
	var cfg Config
	err = yaml.Unmarshal(data, &cfg)
	if err != nil {
		return nil, err
	}
	return &cfg, nil
}

// Giám sát tất cả process
func monitor(cfg *Config) {
	for {
		for group, processes := range cfg.Groups {
			for _, name := range processes {
				if isRunning(name) {
					processStatus.WithLabelValues(group, name).Set(1)
				} else {
					processStatus.WithLabelValues(group, name).Set(0)
				}
			}
		}
		time.Sleep(10 * time.Second)
	}
}

func main() {
	cfg, err := readConfig("config.yaml")
	if err != nil {
		log.Fatalf("❌ Lỗi đọc config.yaml: %v", err)
	}

	go monitor(cfg)

	http.Handle("/metrics", promhttp.Handler())
	fmt.Println("✅ Process monitor running on :9101/metrics")
	log.Fatal(http.ListenAndServe(":9101", nil))
}

```

B7: Chạy chương trình
```
go run process_monitor.go
```

B8: Kiểm tra kết quả

Truy cập vào http://localhost:9101/metrics

```
linux_process_up{group="web",process_name="nginx"} 1
linux_process_up{group="db",process_name="postgres"} 0
```

B9: Truy vấn trong Grafana
```
linux_process_up
linux_process_up{group="web"}
linux_process_up{process_name="nginx"}
```

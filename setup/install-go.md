# Ubuntu
```
wget https://go.dev/dl/go1.22.12.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.22.12.linux-amd64.tar.gz
```

Thêm PATH

Tài khoản root
```
echo 'export PATH=/usr/local/go/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

User khác
```
echo 'export PATH=/usr/local/go/bin:$PATH' >> ~/.profile
source ~/.profile
```

Áp dụng toàn bộ user
```
echo 'export PATH=/usr/local/go/bin:$PATH' | sudo tee /etc/profile.d/go.sh
source /etc/profile.d/go.sh
```

Kiểm tra
```
go version
```




# 🛡️ Configure Named Standard IPv4 ACLs

## 📌 Giới thiệu
Dự án này mô phỏng việc cấu hình và áp dụng **Access Control Lists (ACLs)** trên router để bảo vệ **File Server**.  
Trước khi triển khai ACL, toàn bộ mạng được kiểm tra đảm bảo có thể **ping thành công giữa tất cả các thiết bị**.  

---
## 🌐 Sơ đồ mạng
<p align="center">
<img width="569" height="312" alt="image" src="https://github.com/user-attachments/assets/cc043f14-c018-4816-b311-4370eb1586b6" />
</p>

## 📋 Bảng địa chỉ IP

<p align="center">
<img width="680" height="258" alt="image" src="https://github.com/user-attachments/assets/2fa547b5-c8b7-441d-ad1c-afe26d37e5eb" />
</p>

---
## 📜 Chính sách mạng

### 🔹 Router R1
- ✅ Chỉ **PC1 (192.168.20.4)** và **WebServer (192.168.100.100)** được phép truy cập **File Server**.  
- 🚫 Tất cả các thiết bị khác **không được phép** truy cập File Server.  
- 📍 ACL được áp dụng ở **outbound interface FastEthernet 0/1** (kết nối đến File Server).  

---

## 🎯 Mục tiêu
- 🔒 Bảo mật File Server bằng cách hạn chế truy cập.  
- 🖥️ Đảm bảo chỉ có **Web Manager PC1** và **Web Server** được phép truy cập.  
- 🌐 Ngăn chặn toàn bộ truy cập khác đến File Server.  

---

## 📝 Các bước thực hiện
1. **Kiểm tra kết nối**  
   - Ping từ tất cả các thiết bị đến File Server để xác minh kết nối đầy đủ trước khi áp dụng ACL.  

2. **Cấu hình ACL trên R1**  
   - Tạo ACL chuẩn có tên **File_Server_Restrictions**.  
   - Cho phép truy cập từ **PC1 (192.168.20.4)**.  
   - Cho phép truy cập từ **WebServer (192.168.100.100)**.  
   - Chặn tất cả các truy cập khác.  
   - Áp dụng ACL **outbound** trên interface **FastEthernet 0/1**.  

   ```bash
   R1(config)# ip access-list standard File_Server_Restrictions
   R1(config-std-nacl)# permit host 192.168.20.4
   R1(config-std-nacl)# permit host 192.168.100.100
   R1(config-std-nacl)# deny any

   R1(config)# interface FastEthernet0/1
   R1(config-if)# ip access-group File_Server_Restrictions out


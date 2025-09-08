# 🔒 Configure Numbered Standard IPv4 ACLs
## 📌 Giới thiệu
Dự án này mô phỏng việc cấu hình và áp dụng **Access Control Lists (ACLs)** trên các router để thực thi chính sách mạng.  
Trước khi triển khai ACL, toàn bộ mạng được kiểm tra đảm bảo có thể **ping thành công giữa tất cả các thiết bị**.  

---
## 🌐 Sơ đồ mạng
<p align="center">
  <img width="600" height="437" alt="image" src="https://github.com/user-attachments/assets/e968a8fd-6d5a-41cc-8d32-93444678d410" />
</p>

## **📋 Bảng địa chỉ IP**
<p align="center">
   <img width="679" height="382" alt="image" src="https://github.com/user-attachments/assets/660491d2-360f-4fd1-a7f9-8e5121373860" />
</p>

---


## 📜 Chính sách mạng

### 🔹 Router R2
- 🚫 Mạng **`192.168.11.0/24`** **không được phép truy cập** WebServer tại **`192.168.20.254`**.  
- ✅ Tất cả các truy cập khác **được phép**.  
- 📍 ACL được áp dụng ở **outbound interface hướng đến WebServer**.  

### 🔹 Router R3
- 🚫 Mạng **`192.168.10.0/24`** **không được phép giao tiếp** với mạng **`192.168.30.0/24`**.  
- ✅ Tất cả các truy cập khác **được phép**.  
- 📍 ACL được áp dụng ở **outbound interface hướng đến PC3**.  

---

## 🎯 Mục tiêu
- 🔎 Xác minh kết nối mạng **trước khi áp dụng ACL**.  
- ⚙️ Cấu hình ACL chuẩn để thực hiện **đúng chính sách bảo mật**.  
- 🌐 Đảm bảo **không ảnh hưởng** đến các lưu lượng khác ngoài chính sách đã định.  

---

## 📝 Các bước thực hiện
1. **Kiểm tra kết nối**  
   - Ping từ một PC đến tất cả các thiết bị trong mạng để xác minh kết nối đầy đủ.  

2. **Cấu hình ACL trên R2**  
   - Tạo rule chặn truy cập từ **`192.168.11.0/24` → `192.168.20.254`**.  
   - Tạo rule cho phép toàn bộ các lưu lượng khác.  
   - Áp dụng ACL trên **outbound interface đến WebServer**.  

3. **Cấu hình ACL trên R3**  
   - Tạo rule chặn truy cập từ **`192.168.10.0/24` → `192.168.30.0/24`**.  
   - Tạo rule cho phép toàn bộ các lưu lượng khác.  
   - Áp dụng ACL trên **outbound interface đến PC3**.  

---

## ✅ Kết quả mong đợi
- ❌ Mạng **`192.168.11.0/24`** không thể truy cập WebServer (**`192.168.20.254`**).  
- ❌ Mạng **`192.168.10.0/24`** không thể giao tiếp với mạng **`192.168.30.0/24`**.  
- ✅ Các lưu lượng khác hoạt động **bình thường**.  

---

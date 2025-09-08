# 🛠️ HSRP-OSPF-STP-Lab

## 📌 Giới thiệu
Dự án này mô phỏng một **Campus Network** với các công nghệ quan trọng trong CCNP bao gồm:

- 🔄 **HSRP (Hot Standby Router Protocol)** – dự phòng gateway cho các VLAN.  
- 🛰️ **OSPF (Open Shortest Path First)** – định tuyến động giữa các router.  
- 🌳 **STP (Spanning Tree Protocol)** – loại bỏ vòng lặp Layer 2.  
- 📡 **DHCP** – cấp phát IP động cho các máy trạm trong VLAN.  
- 🌍 **NAT Overload (PAT)** – cho phép các PC trong VLAN truy cập Internet qua ISP.  

🎯 **Mục tiêu**: Xây dựng một mô hình **mạng doanh nghiệp có tính dự phòng cao, định tuyến động và tối ưu hóa truy cập Internet**.

---

## 🌐 Sơ đồ mạng
<img width="891" height="656" alt="image" src="https://github.com/user-attachments/assets/6ae2e463-c7a5-416c-9104-03e90cb9d228" />


---

## 📋 Thông số VLAN
| VLAN | Subnet            | Virtual IP (HSRP) | Active Router | Root Bridge (STP) |
|------|------------------|-------------------|---------------|-------------------|
| 10   | 192.168.10.0/24  | 192.168.10.254    | CORE_251      | CORE_251          |
| 20   | 192.168.20.0/24  | 192.168.20.254    | CORE_251      | CORE_251          |
| 30   | 192.168.30.0/24  | 192.168.30.254    | CORE_252      | CORE_252          |
| 40   | 192.168.40.0/24  | 192.168.40.254    | CORE_252      | CORE_252          |

---

## ⚙️ Thành phần cấu hình

### 🔹 1. VLAN & Trunk
- Các Access Switch: chia port theo VLAN 10–40.  
- Kết nối lên Core Switch dùng **802.1Q trunk**.  

### 🔹 2. HSRP
- CORE_251 Active cho VLAN **10, 20**.  
- CORE_252 Active cho VLAN **30, 40**.  
- Virtual IP là Default Gateway của client.  

### 🔹 3. STP
- CORE_251 là **Root Bridge** cho VLAN **10, 20**.  
- CORE_252 là **Root Bridge** cho VLAN **30, 40**.  

### 🔹 4. OSPF Routing
- OSPF Process ID: `100`  
- Area: `1`  
- Mạng quảng bá:  
  - Các VLAN (192.168.x.0/24)  
  - Liên kết Core ↔ WAN Router (172.16.x.0/24)  
  - Liên kết WAN ↔ ISP (112.1.1.0/24)  
- WAN_R dùng `default-information originate` để quảng bá đường ra Internet.  

### 🔹 5. DHCP
- CORE Switch cấp phát DHCP cho VLAN 10–40.  
- Default Gateway là **Virtual IP HSRP**.  
- DNS mặc định: `8.8.8.8`.  
- Địa chỉ Virtual IP & địa chỉ thực của CORE được loại trừ (`excluded-address`).  

### 🔹 6. NAT Overload
- WAN_R thực hiện **NAT Overload** (PAT).  
- ACL 10 cho phép NAT toàn bộ mạng nội bộ.  
- Inside: G0/0–1 (172.16.10.2 / 172.16.20.2).  
- Outside: G0/2 (112.1.1.1).  

---

## ✅ Kết quả kiểm thử
- 🖥️ PC trong VLAN **nhận IP động** từ DHCP.  
- 📡 Default Gateway của PC là **Virtual IP (HSRP)**.  
- 🔄 Khi tắt một Core Switch, các PC **vẫn truy cập Internet** bình thường nhờ HSRP.  
- 📍 OSPF đảm bảo **định tuyến động, hội tụ nhanh** khi có sự cố.  
- 🌍 NAT Overload trên WAN_R cho phép PC ping/truy cập **8.8.8.8 (Internet)** thành công.  

---

## 🚀 Hướng mở rộng
- Thêm **VRRP/GLBP** để so sánh với HSRP.  
- Kết hợp **ACL nâng cao** để quản lý truy cập giữa các VLAN.  
- Thêm **Syslog & SNMP** để giám sát mạng.  
- Tích hợp **HSRP Tracking** để giám sát link WAN.  

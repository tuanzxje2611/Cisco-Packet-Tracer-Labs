# 🔐 GRE-VPN-SITE2SITE

## 📌 Giới thiệu
Dự án này mô phỏng một **Enterprise WAN Network** kết nối nhiều chi nhánh thông qua **GRE VPN Tunnel** kết hợp với **OSPF định tuyến động**. Ngoài ra còn có các công nghệ:

- 🌐 **GRE VPN (Generic Routing Encapsulation)** – kết nối site-to-site qua ISP.  
- 🛰️ **OSPF (Open Shortest Path First)** – định tuyến động giữa các site và ISP.  
- 📡 **DHCP** – cấp phát IP động cho các máy trạm.  
- 🌍 **NAT Overload (PAT)** – cho phép các VLAN truy cập Internet thông qua ISP.  
- 🔀 **VLAN & Inter-VLAN Routing** – phân chia mạng nội bộ theo phòng ban.  

🎯 **Mục tiêu**: Xây dựng một hệ thống **Site-to-Site VPN** cho doanh nghiệp, kết nối các chi nhánh (Saigon, Can Tho, Vung Tau) qua ISP, có **khả năng định tuyến động, bảo mật và dễ mở rộng**.

---

## 🌐 Sơ đồ mạng
<img width="1618" height="718" alt="image" src="https://github.com/user-attachments/assets/823b07eb-7095-4224-8ae6-218b673b2111" />

---

## 📋 Thông số mạng
| Site        | VLAN/Subnet            | Gateway (SVI/Router) | DHCP/NAT | Ghi chú |
|-------------|------------------------|-----------------------|----------|---------|
| **Site 1 – Saigon (HQ)**  | VLAN10: 172.16.10.0/24<br>VLAN20: 172.16.20.0/24<br>VLAN30: 172.16.30.0/24 | 172.16.x.254 | DHCP tại Server | Core-SW1 làm Inter-VLAN Routing |
| **Site 2 – Can Tho**     | VLAN10: 172.17.10.0/24<br>VLAN20: 172.17.20.0/24 | 172.17.x.254 | DHCP tại Router | Router-on-a-Stick |
| **Site 3 – Vung Tau**    | VLAN10: 172.18.10.0/24<br>VLAN20: 172.18.20.0/24<br>VLAN30: 172.18.30.0/24 | 172.18.x.254 | DHCP tại Core SW | Layer 3 Switch cấp phát DHCP |
| **ISP (R1-R2-R3)**       | Backbone: 11.1.1.0/30, 12.1.1.0/30, 13.1.1.0/30, 22.1.1.0/30, 23.1.1.0/30, 33.1.1.0/30 | /30 Point-to-Point | N/A | Chạy OSPF, đóng vai trò trung gian |

---

## ⚙️ Thành phần cấu hình

### 🔹 1. VLAN & Routing nội bộ
- Saigon: Core-SW1 làm Inter-VLAN Routing.  
- Can Tho: Router-on-a-Stick trên R-WAN-S2.  
- Vung Tau: Core-SW1-S3 là L3 Switch cấp phát DHCP & định tuyến VLAN.  

### 🔹 2. GRE VPN Tunnels
- **Tunnel 12**: Saigon ↔ Can Tho (192.168.12.0/24).  
- **Tunnel 13**: Saigon ↔ Vung Tau (192.168.13.0/24).  
- Tunnels được quảng bá vào OSPF.  

### 🔹 3. OSPF Routing
- OSPF Process ID: `100`  
- Area: `0`  
- Quảng bá:  
  - Các VLAN trong mỗi site.  
  - Các mạng GRE Tunnel (192.168.12.0/24, 192.168.13.0/24).  
  - Các kết nối backbone ISP (/30).  
- ISP Router chạy OSPF, học toàn bộ route của các site.  

### 🔹 4. DHCP
- **Saigon**: DHCP Server cấp phát cho VLAN10, 20.  
- **Can Tho**: Router cấp phát DHCP cho VLAN10, 20.  
- **Vung Tau**: Core-SW1-S3 cấp phát DHCP cho VLAN10, 20, 30.  

### 🔹 5. NAT Overload
- R-WAN-S1 & R-WAN-S2 & R-WAN-S3 đều cấu hình **NAT Overload** để truy cập Internet.  
- ACL 10 permit any → overload trên interface WAN kết nối ISP.  

---

## ✅ Kết quả kiểm thử
- 🖥️ PC tại mỗi site **nhận IP động** từ DHCP.  
- 🔗 Các site **liên lạc với nhau qua GRE VPN Tunnel**.  
- 🛰️ OSPF học route động giữa HQ ↔ Chi nhánh ↔ ISP.  
- 📡 NAT Overload cho phép tất cả PC truy cập Internet qua ISP.  
- 🔄 Nếu 1 đường đi qua ISP bị down, OSPF hội tụ nhanh sang đường còn lại.  

---

## 🚀 Hướng mở rộng
- Thêm **IPSec** để bảo mật GRE Tunnel.  
- So sánh **EIGRP vs OSPF** trong định tuyến đa site.  
- Kết hợp **HSRP/VRRP** tại HQ để dự phòng gateway.  
- Thêm **QoS** để ưu tiên VoIP hoặc ứng dụng quan trọng.  

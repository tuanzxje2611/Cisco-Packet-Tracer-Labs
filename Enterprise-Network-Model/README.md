# 🌐 Network Project – Company A & Company B
## 🖥️ Topology
<img width="962" height="568" alt="image" src="https://github.com/user-attachments/assets/294ad064-4d0b-4f74-9c29-974501f78c82" />

## 🎯 Objectives
Xây dựng hệ thống mạng cho *Công ty A* và *Công ty B*, đảm bảo:
- Quản lý người dùng theo VLAN.
- Định tuyến động bằng *EIGRP* (Công ty A) và *RIP v2* (Công ty B).
- *DHCP* cấp phát IP tự động.
- *NAT/PAT* để truy cập Internet.
- Public dịch vụ nội bộ (Web/Mail).
- Kết nối ISP chạy *OSPF* để ra Internet.

---

## 🏢 Công ty A
- *Switch SW0, SW1, SW2*: tạo VLAN 10–50 cho các phòng ban.  
- *Router R1, R4, R5*:
  - Cấu hình *Router-on-a-Stick* với dot1Q subinterface.
  - Chạy *EIGRP AS 100* quảng bá các mạng LAN (172.16.x.0/24) & WAN (192.168.x.0/24).
- *DHCP Relay* (ip helper-address) chuyển tiếp yêu cầu về DHCP Server tại VLAN 50.  
- *Router R6 (Edge Router)*:
  - *NAT Overload* cho người dùng.
  - *Static NAT* cho Web/Mail Server ra ngoài (TCP 80/25).
  - Quảng bá *default route* trong EIGRP (redistribute static).

---

## 🏢 Công ty B
- *Switch SW1, SW2*: VLAN 10–40.  
- *SW_CORE1, SW_CORE2 (Switch L3)*:
  - *Inter-VLAN routing* (ip routing).
  - DHCP Server tại *SW_CORE1*, *SW_CORE2* dùng relay.
  - Kết nối bằng *EtherChannel (Port-Channel 3)*, IP riêng trên link (172.17.12.0/24).
- *Router R1 (Edge)*:
  - Kết nối ISP qua 111.1.2.0/30.
  - Chạy *RIP v2* để quảng bá các VLAN & mạng với MLS.
  - *NAT Overload* cho người dùng ra Internet.
  - Phát *default route* trong RIP (default-information originate).

---

## 🌐 ISP
- Các router ISP chạy *OSPF area 1*.  
- *R_ISP5*, *R_ISP1* phát *default route* cho toàn mạng ISP.  
- *8.8.8.8* DNS server công cộng.  
- ISP đảm bảo kết nối ra Internet toàn cầu.

---

## ⚙️ Implemented Services
- VLAN segmentation.  
- *EIGRP* & *RIP v2* nội bộ.  
- *OSPF* trong ISP.  
- *DHCP* cấp IP tự động.  
- *NAT/PAT* cho Internet.  
- *Static NAT* cho Web/Mail server.  
- *EtherChannel* SW_CORE1 và SW_CORE2.  
- *Default route propagation* toàn hệ thống.

---

📌 README này mô tả kiến trúc, công nghệ và dịch vụ trong dự án mạng.  

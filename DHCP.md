# 1. DHCP là gì?

Dynamic Host Configuration Protocol (DHCP) gán địa chỉ IP cho các thiết bị.

DHCP có 3 cách cấp phát địa chỉ IP
- Cấp phát thủ công: DHCP xác định trước địa chỉ IP cho client
- Cấp phát tự động: DHCP tự động gán địa chỉ IP từ pool
- Cấp phát động: DHCP tự động gán, thu hồi và cấp phát trở lại các IP từ pool

Quá trình xử lý cấp phát IP
- DHCPDISCOVER
- DHCPOFFER
- DHCPREQUEST
- DHCP ACK

# 2. Cấu hình DHCPv4

- Tạo pool các địa chỉ không được phép cấp:
```sh
# ip dhcp excluded-address 192.168.1.1 192.168.1.10
# ip dhcp excluded-address 192.168.1.254
```

- Tạo một pool name
```sh
# ip dhcp pool LAN-POOL-1
# network 192.168.1.0 255.255.255.0
# default router 192.168.1.1
# dns-server 192.168.11.2
# domain-name example.com
```

- Kiểm tra lại cấu hình DHCP
```sh
# show running-config | sestion dhcp
# show ip dhcp binding
# show ip dhcp server statistics
```

- Bằng cách sử dụng kỹ thuật helper để chuyển tiếp gói tin DHCP broadcast qua router.
```sh
# interface g0/0
# ip helper-address 192.168.2.10
```

- Cấu hình 1 port router như một DHCP client
```sh
# interface g0/1
# ip address dhcp
# no shutdown
```

- Kiểm tra cấu hình DHCPv4 của Router:
```sh
# show running-config | section interface G0/0

hoặc
# show running-config | include no service dhcp
```

# 3. Cấu hình SLAAC và DHCPv6

SLAAC: Stateless address autoconfiguration

- Cấu hình một Router như một Stateless DHCPv6 Server
```sh
R1(config)# ipv6 unicast-routing
R1(config)# ipv6 dhcp pool IPV6-STATELESS
R1(config-dhcpv6)# dns-server 2001:x::x
R1(config-dhcpv6)# domain-name example.com
R1(config-dhcpv6)# exit
R1(config)# interface g0/1
R1(config-if)# ipv6 address 2001:x::y/64
R1(config-if)# ipv6 dhcp server IPV6-STATELESS
R1(config-if)# ipv6 nd other-config-flag
```

- Cấu hình 1 port trên Router như một Stateless DHCPv6 Client
```sh
R3(config)# interface f0/1
R3(config-if)# ipv6 enable
R3(config-if)# ipv6 address autoconfig
```

- Kiểm tra lại thông tin cấu hình:
```sh
# show ipv6 interface
```

- Cấu hình Router như một stateful DHCPv6 Server
```sh

```
# IP

## 주요 헤더

* TTL
    * 라우터가 전달할 때마다 값을 1씩 깎음
    * TTL이 0이면 전송 중지

* Protocol: 데이터가 어떤 프로토콜인지 표시
    * 1: ICMP
    * 2: IGMP
    * 6: TCP
    * 17: UTP

* Option: 기타 정보를 담고 있음
    * Loose Source Routing: Option에 기록된 라우터를 거쳐서 통신
    * Strict Source Routing: Option에 기록된 라우터만 이용해야 함

# CDN

* 외국 서버의 정보를 국내 IDC에 설치하고 그 주소를 외국 서버에 등록해 데이터를 저장해 놓음

# ICMP

* 라우터끼리 통신하는 프로토콜

## 주요 메시지

* Echo/Eco Reply
* Destinatino Unreachable
* Router Solicitation: 라우터 정보 요청
* Time Exceeded
* Parameter Problem
* Address Mask Request: 서브넷 마스크 요청
* Address Mask Reply: 서브넷 마스크 응답

# BOOTP, DHCP

## BOOTP

* 과거에는 하드웨어 기능이 부족해 통신으로 부팅 소스를 받아오는 경우가 있었음
* RAPR보다 더 많은 정보를 전달해줌
* IP-MAC 매핑을 정적으로 관리함

## DHCP

* BOOTP + 동적 관리

### DHCP 메시지

* DHCPDISCOVER
* DHCPOFFER
* DHCPREQUEST
* DHCPACK
* DHCPNACK
* DHCPDECLINE
* DHCPRELEASE: IP 반납(동적 관리 가능해짐)

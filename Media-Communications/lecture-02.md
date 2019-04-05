# 이더넷

* 1978년 도입
* MAC Address를 이용한 통신
    * MAC Address는 하나의 스위치 안에서 고유한 값을 가짐

# ARP

* STD37, RFC826
* IP -> MAC

# RARP

* MAC -> IP
* DHCP가 있기 때문에 거의 쓰지 않음

# Multicast, Unicast, Broadcast

## Multicast

* IGMP 프로토콜을 활용해 라우터 조인
* 224~239 IP를 사용함
* 최종단 라우터가 데이터를 보냄

## Unicast

* 소스 주소에서 데이터를 직접 보냄
* Multicast보다 부하가 큼

## Broadcast

* 내 네트워크 안의 모든 단말에 신호 전송

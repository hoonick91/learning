L4, L7 Loadbalancer

**[요청 전달 시 변조]**

사용자 → L4 → NAT(IP/MAC 주소 변조) → real server
→ 사용자가 L4를 호출하면 중간에 NAT가 목적지 IP 주소를 real server IP 주소로 변조하고 MAC 주소도 변조한다.

**[응답 전달 시 변조]**

real server → NAT → L4 → 사용자
— -> real server에서 L4를 거치면서 출발지(source) IP 주소를 L4 가상 IP 주소로 변조한다. 동일 네트워크 대역이므로 MAC 주소는 변조하지 않는다.

**NAT** : 네트워크 주소 변환(NAT) 이를 사용하는 이유는 하나의 공인 IP를 사용하여 여러 개의 호스트가 인터넷에 접속하기 위해서 이다.(사설 IP주소를 사용)

참조

[https://medium.com/@pakss328/%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%84%9C%EB%9E%80-l4-l7-501fd904cf05](https://medium.com/@pakss328/로드밸런서란-l4-l7-501fd904cf05)
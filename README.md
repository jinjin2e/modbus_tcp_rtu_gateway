# modbus_tcp_rtu_gateway

할 일   


## TCP -> RTU
Modbus TCP 헤더 (7 bytes) 제거:  

Transaction ID (2 bytes)  
Protocol ID (2 bytes)  
Length (2 bytes)  
Unit ID (1 byte)  
RTU 패킷 생성:  

Modbus TCP 패킷의 PDU (Protocol Data Unit) 부분만 사용하여 RTU 패킷을 생성.  
RTU에서는 CRC (Cyclic Redundancy Check) 체크섬이 필요함.  

## RTU -> TCP  

Modbus RTU 패킷의 CRC 제거:  

Modbus RTU 응답 데이터의 끝에 있는 2바이트 CRC 체크섬을 제거합니다.  
TCP 헤더 추가:

Transaction ID, Protocol ID, Length, Unit ID와 함께 RTU 패킷을 TCP 패킷으로 변환합니다.  


## 예시

Modbus TCP 요청 -> Modbus RTU 요청  
Modbus TCP 요청 예시:  

00 01 00 00 00 06 01 03 00 6B 00 03   
Modbus RTU 요청 변환 과정:  

TCP 헤더 제거: 03 00 6B 00 03  
CRC 계산 및 추가: 예를 들어, CRC가 7E 1C라고 가정하면,  
최종 RTU 요청: 01 03 00 6B 00 03 7E 1C  
Modbus RTU 응답 -> Modbus TCP 응답  
Modbus RTU 응답 예시:  

01 03 06 02 2B 00 00 00 00 F8 4A  
Modbus TCP 응답 변환 과정:  

CRC 제거: 01 03 06 02 2B 00 00 00 00  
TCP 헤더 추가: 예를 들어, Transaction ID와 Protocol ID를 포함시키면,  
최종 TCP 응답: 00 01 00 00 00 07 01 03 06 02 2B 00 00 00 00  
 

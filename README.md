## secureCRT에서 linux 2023 AMI로 생성한 EC2 접속 실패
이슈
- secureCRT에서 pem key로 EC2에 접속 요청 시 failure 발생  

해결
- 기존에는 EC2 생성 시 aws amazon linux2로 생성했는데 이번 이슈에서는 linux 2023 AMI으로 생성했음.  
- linux 2023 AMI로 생성한 서버는 /etc/ssh/sshd_config 파일 설정값이 amazon linux2로 생성한 서버와 다름(업데이트가 된걸로 추측)

## open vpn를 통해 aws vpc에 접속했을 때 피어링 된 타 vpc에 접속이 불가능한 현상
이슈
- 랜선이 연결된 로컬 PC에서 Open VPN을 통해 AWS VPC에 접속 완료하여 IP 할당받은 후, 접속한 VPC와 피어링된 VPC에 접속 시도 시 실패
- 와이파이를 사용하는 맥북은 피어링된 VPC까지 접속이 잘 됨

해결
- Open VPN 네트워크와 이더넷 네트워크의 메트릭 값이 자동으로 설정되어 있었음.
- 자동을 해제하고 수동으로 메트릭 값을 설정하여 Open VPN이 이더넷보다 우선순위가 높도록 처리(Open VPN: 1, 이더넷: 100)

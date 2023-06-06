## secureCRT에서 linux 2023 AMI로 생성한 EC2 접속 실패
이슈
- secureCRT에서 pem key로 EC2에 접속 요청 시 failure 발생
---
해결
- 기존에는 EC2 생성 시 aws amazon linux2로 생성했는데 이번 이슈에서는 linux 2023 AMI으로 생성했음.
- linux 2023 AMI로 생성한 서버는 /etc/ssh/sshd_config 파일 설정값이 amazon linux2로 생성한 서버와 다름(업데이트가 된걸로 추측)
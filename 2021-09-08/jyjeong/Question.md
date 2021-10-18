# Question

**#. Service(proxy)는 어디서 관리 ?**

쿠버네티스 클러스터 <br>
그냥 마음으로 받아들이면 될 듯 <br>

<br>

**#. Service 내 port, target-port / Deployment 내 Container-port**

Service / target-port : 내부 Pod-Container와 통신하기 위한 port 정의 <br>
Service / port :  <br>
Deployment / Container-port : 내부 Service와 통신 <br>
→ Service:target-prot, Deployment:Container-port 맞추는게 국룰이다. <br>
Selector 같이 맞춰야함. <br>

<br>

**#. Docker image가 k8s cluster 내부에 존재?**

Image Registry는 Habor 그 자체이다. <br>
현재 구조(13.124.61.239)에서는 Docker hub <br>
우리가 docker images로 보는 건 local disk에 cache된 이미지이다. <br>
Cache Image 개념이랑 Image Registry 개념은 다르다. <br>
(Node마다 cache) <br>

**#. Server 구조**

H/W, Kernel, Shell, OS              <br>
ex) k exec -it {podName} — bash     <br>
→ bash의 의미?                      <br>

<br>

**H/W, OS, Kernel** <br>
**→ bash는 prompt를 띄어주기 위한 명령어일뿐이고** <br>
**kernel을 공유하는 건 무조건 default이다.** <br>

<br>

**#. 보통 Spring Boot를 pod에 올릴 때, jar,war파일을 올려서 실행하는 구조 ?** 

거의 국룰이다. <br>
.jar가 제일 가볍기 때문에 Spring Boot를 .jar로 만들어 배포-서비스한다. <br>
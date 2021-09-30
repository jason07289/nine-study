# OKD CI/CD

## 개발자 소스 커밋
![image](https://user-images.githubusercontent.com/38865267/135419128-71d6a4c7-eea3-4632-b074-87163a8ac815.png)

- jenkins에는 git(소스 받기), maven/gradle(자바빌드), Cri-o(이미지 생성) , Argocd - cli(B/G 배포)가 설치되어 있어야 한다.
<br/>

1. 개발자가 gitlab으로 tag를 push 한다.
   
2. jenkins는 소스가 올라오면 gitlab의 소스를 clone 한다.
   
3. jenkins에서 소스(java)를 build 한다.
   
4. jenkins는 containerizing(Dockerfile에 써서 실제 이미지를 생성)하고 이미지를 harbor로 push 한다.
   
5. jenkins가 이미지를 OKD에 pod 형태로 deployment 한다. (배포) 

6. Argocd에서 blue/green 형식으로 올라온 파드에 대해 실제 운영서버에 배포가 가능하다.


<br/><br/>

## Blue / Green 무중단 배포

 ![image](https://user-images.githubusercontent.com/38865267/135421315-2f5c09f6-f886-4add-998b-352b10ed033a.png)
- as-is service 운영 중


<br/>

![image](https://user-images.githubusercontent.com/38865267/135421843-35905789-9caa-4e04-9cc7-b47330951b89.png)

- OKD 내에 새로운 버전의 pod v2가 들어온 상황 (테스트 서버에 연결되어 있다.) 회사 내부에서 개발자/현업 테스트 가능


<br/>

![image](https://user-images.githubusercontent.com/38865267/135422475-53cc996a-43da-48bb-88f2-ec96494797a8.png)
- 테스트 완료시 POD v2를 실 운영중인 운영1로 연결


<br/>


![image](https://user-images.githubusercontent.com/38865267/135423020-4ad88f56-474c-4c22-98f5-c65a19d61b4d.png)

- B/G 배포에서는 이전 버전의 POD가 관리자가 설정한 시간동안 남아있다. 만약, 운영중 문제 발생시 남아있던 이전 버전으로 원복가능하다.


- 이전 버전이 남아있지 않는 경우에는 원복은 가능하나 이미지를 다시 띄워야 해서 시간이 좀 더 걸린다.

- 크리티컬한 경우엔 canari가 더 알맞다. (특정 사용자에게 테스트 가능) But, 이커머스 쪽에선 B/G를 많이 쓴다.

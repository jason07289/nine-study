# Docker 환경에서 jar 파일 배포 remind

## 배포하고자 하는 Pod
- spring boot sample 파일 구동이 목표

  ![image](https://user-images.githubusercontent.com/38865267/133012516-097419d2-68c9-43e7-8d50-795110bf9322.png)


## K8s,Docker 환경에서 배포
- 서비스하고자하는 파일(taco.jar)을 git에서 받고 해당 환경에 맞는 openjdk image를 도커 hub에서 찾는다.
 ![image](https://user-images.githubusercontent.com/38865267/133013745-26302e27-b8dc-4b0e-a7a8-899b04edf391.png)

- openjdk 위에 taco.jar파일이 올라간 이미지를 만들기 위해 Dockerfile을 작성한다.
 ![image](https://user-images.githubusercontent.com/38865267/133014199-e5cbe084-7a4c-4bd5-a373-2b5e0803d3ae.png)
- Dockerfile
 
  ![image](https://user-images.githubusercontent.com/38865267/133015319-a37a735c-efc1-42c3-be29-bc59bd7930bc.png)

- "docker build -t [image명]:[tag]"로 이미지 생성
- 로컬에 도커이미지가 생성된 것을 확인할 수 있었다.
  ![image](https://user-images.githubusercontent.com/38865267/133026514-7641dfa2-7fad-4aff-a66e-574dfb371390.png)

- 도커이미지 공유, ci/cd를 위해서라면 docker hub repository에 push 작업도 필요하다.
 ![image](https://user-images.githubusercontent.com/38865267/133027100-6d4fddaa-9505-47ce-becc-72b5110427cb.png)

- 


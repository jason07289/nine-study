# Docker 환경에서 jar 파일 배포 remind

## 배포하고자 하는 Pod
- spring boot sample 파일 구동이 목표<br/>
  ![image](https://user-images.githubusercontent.com/38865267/133012516-097419d2-68c9-43e7-8d50-795110bf9322.png)


## K8s,Docker 환경에서 배포
- 서비스하고자하는 파일(taco.jar)을 git에서 받고 해당 환경에 맞는 openjdk image를 도커 hub에서 찾는다.<br/>
 ![image](https://user-images.githubusercontent.com/38865267/133013745-26302e27-b8dc-4b0e-a7a8-899b04edf391.png)

- openjdk 위에 taco.jar파일이 올라간 이미지를 만들기 위해 Dockerfile을 작성한다. <br/>
 ![image](https://user-images.githubusercontent.com/38865267/133014199-e5cbe084-7a4c-4bd5-a373-2b5e0803d3ae.png)
- Dockerfile <br/>
  ![image](https://user-images.githubusercontent.com/38865267/133015319-a37a735c-efc1-42c3-be29-bc59bd7930bc.png)

- "docker build -t [image명]:[tag]"로 이미지 생성
- 로컬에 도커이미지가 생성된 것을 확인할 수 있었다. <br/>
  ![image](https://user-images.githubusercontent.com/38865267/133026514-7641dfa2-7fad-4aff-a66e-574dfb371390.png)

- 도커이미지 공유, ci/cd를 위해서라면 docker hub repository에 push 작업도 필요하다. 
- image명은 [계정명]/[이미지명]:[태그] 이런 구조가 되어야함. <br/>
 ![image](https://user-images.githubusercontent.com/38865267/133027100-6d4fddaa-9505-47ce-becc-72b5110427cb.png)

- pod 생성을 위해 deployment.yaml 파일을 작성한다. (POD배포 정의 파일) <br/>
 ![image](https://user-images.githubusercontent.com/38865267/133028824-f52cddde-5ebf-42c9-bca8-f029898f24f8.png)
- deployment.yaml 파일 주요 정보
  - kind: deployment를 만들겠다.
  - metadata
    - name: 해당 필드에 따라 deployment의 이름이 결정된다.
  - spec.replicas: 레플리카 Pod의 개수
  - spec.selector: deployment가 관리할 Pod를 찾는 방법을 정의한다. 
  - template
    - selector에서 정의한 label은 metadata.label에 정의 된 taco-jh을 선택하게 되어있다.
    - spec.containers
      - image: 이미지 이름
      - ports.containerPort: 내부 서비스와 통신할 port, ** Service:target-prot, Deployment:Container-port 맞추는게 국룰이다.

- 이제 kubectl명령어로 deployment를 생성한다.
- 위 deployment.yaml로 설정한 이름의 deployment가 이제 동작한다.
- replica의 개수 만큼 pod가 생성되며 고유의 주소를 가진다. <br/>
 ![image](https://user-images.githubusercontent.com/38865267/133038746-4bbd8d23-194c-4d20-b917-a93f697a10c1.png)

- 앞에서 생성한 pod들은 ip가 유동적으로 변경되기에 고정된 엔드포인트로 접근하기는 상당히 힘들다. 또한 여러 pod가 있을 경우 pod 간 로드밸런싱이 필요한데 이를 위해 Service가 존재한다.
- 해당 pod들을 연결하기 위해 service(proxy)를 만들어야 하므로 service.yaml 파일을 만든다.<br/>
 ![image](https://user-images.githubusercontent.com/38865267/133039304-72197f72-3008-4d45-89b7-dc52e2f6ae98.png)

- service.yaml 파일 주요 정보
  - kind: service를 만들겠다.
  - metadata.name: service명
  - spec
    - type: 연결 타입 
     - ClusterIP: 서비스에 클러스터 IP(내부 IP)를 할당, 외부 IP가 없기에 k8s 클러스터 내에선 접근이 가능하지만 외부에서 접근 불가.
    - NodePort: 모든노드의 ip와 포트를 통해 접근이 가능.
    - ports.port: 접속할 Port
    - ports.targetPort: 내부 Pod-Container와 통신하기 위한 port 정의
    - ports.nodePort: type이 NodePort일 때, 이걸 설정하면 기본 ports.port로도 접속이 가능하지만 nodePort도 client가 접속이 가능! 
     - 참고사진 <br/>
      ![image](https://user-images.githubusercontent.com/38865267/133043284-168078d8-081b-4cf6-be86-3e464bb14297.png)![image](https://user-images.githubusercontent.com/38865267/133043684-cd2e667a-8c53-49f8-a762-08ca68875e0d.png) <br/>
      출처: https://bcho.tistory.com/1262
- kubectl 명령어로 service를 생성
- kubectl get svc로 생성된 서비스를 확인할 수 있다. <br> ![image](https://user-images.githubusercontent.com/38865267/133047583-b5b50337-43ec-41de-9cab-d971782c31f5.png)
- service에 명시된 외부 포트로 client가 접속 가능
  <br>![image](https://user-images.githubusercontent.com/38865267/133045225-a9ef82fe-3ac1-4da4-ac8c-114cd7008659.png)
  

  
## k8s상에 최종 배포된 구성도
![image](https://user-images.githubusercontent.com/38865267/133048080-6eba6f4d-43b5-4e1f-8d4d-8c3ab1c1470d.png)

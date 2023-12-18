# K8S External Traffic
> 아래 내용은 [여기](https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0) 를 참고해서 정리함
* K8S 에서는 외부 트래픽을 Service 라는 레이어를 통해서 Pod 으로 전달한다.
* 외부 트래픽을 받을 수 있는 방식은 크게 3가지로 나눌 수 있다
  * Proxy
  * NodePort
  * LoadBalancer
  * Ingress

## Proxy
* 프록시로 들어오는 트래픽을
* 특정 포트로 들어오는 트래픽을 모두 전달하는 것이기 때문이 일반적인 방법은 아니다
* SSH 로 서버에 접속하는걸 방지하기 위해 22 포트로 접근할 통로를 막아버렸는데, 긴급한 이유로 외부에서 22 포트로 접속해야 할 때 잠깐 오픈한다던지 아는 케이스에만 사용하고 일반적으로 프로덕션 배포에 사용할 경우는 많지 않다.
* Flow![flow](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*I4j4xaaxsuchdvO66V3lAg.png)

## NodePort
* 특정 포트로 들어오는 트래픽을 수신하는 서비스를 생성
* 30000 ~ 32767 사이의 포트만 설정 가능
* host 상관 없이 포트만 지정하는 것이기 때문에, 호스트별로 트래픽을 라우팅 시켜줄 경우는 적합하지 않다
* 설정 자체도 불가능하긴 하지만 만약 80 포트에 대한 NodePort 를 설정한다면, 웹 트래픽은 모두 한 서비스에 몰리는 꼴이 된다
* Flow![flow](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*CdyUtG-8CfGu2oFC5s0KwA.png)

## Load Balancer
* 트래픽을 받는 로드밸런서 타입 서비스를 생성한다
* 외부로 노출되는 IP 하나가 할당된다
* 해당 IP 로 들어오는 트래픽은 로드밸런서 서비스가 수신하게 된다
* 특정 포트에 종속적인게 아니고 L3 레이어의 리소스 (IP) 만 지정해 주는 것이기에 L4 레이어 (TCP, UDP, ...) 등을 자유롭게 선택할 수 있다
* 로드 밸런서 타입으로 외부 트래픽을 노출할 경우 서비ㅂ마다 공인IP 를 할당받아야 하기에 유지관리 비용이 많이 들 수 있다. (공인 IP 는 상당히 귀하신 분이며 비싸다!)
* Flow![flow](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*P-10bQg_1VheU9DRlvHBTQ.png)

## Ingress
* Ingress 는 사실 Service 타입의 리소스는 아니다
* Ingress 는 Service 보다 상위개념이다.
* NodePort, Load Balancer 에서는 설정이 불가능했던 Host + Port 조합으로 각 서비스에 트래픽을 분산해 줄 수 있고, URL Path 에 따라 트래픽을 분산해주는 보다 다양한 기능을 제공한다.
* 주로 HTTP(S) 트래픽 라우팅을 설정할 때 쓰인다.
  * PATH 단위로 트래픽 라우팅을 설정할 수 있는데, 이는 HTTP 에 기반을 두고 있기 때문에 주로 HTTP 트래픽 라우팅에 쓴다
* HTTP(S) 이외의 트래픽에 대한 서비스를 만들어야 한다면, NodePort 나 Load Balancer 타입으로 서비스를 생성해야 한다
* Flow![flow](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*KIVa4hUVZxg-8Ncabo8pdg.png)

# VPC

## Overall
* Virtual Private Cloud 의 약자
* 여기서 Cloud 는 Network 라고봐도 무방할 만큼 VPC 는 네트워크 관리가 주 목적이다
* GCP 의 VPC 는 프로젝트마다 최소 하나씩은 할당되며 최대 5개까지 생성할 수 있다.
* VPC 자체는 Region, Zone 에 종속적이지 않으며 가상의 네트워크 그룹이라고 보면 됨
  * 다만 VPC 하위의 리소스 (Subnet, Zone, ..) 등은 특정 리전에 속하는 리소스이다.
  * ![VPC 네트워크 예시](https://cloud.google.com/static/vpc/images/vpc-overview-example.svg?hl=ko)

## Subnet
* VPC 피자 한판이라고 했을 때, Subnet 은 목적에 맞게 조각낸 조각피자라고 볼 수 있다.
* `Subnet 은 특정 Regio 의 리소스이다!!`
* IPv(4/6) 주소 대역으로 네트워크를 분할 할 수 있으며, CIDR 표기법으로 IP 대역을 분할한다.
* 아직 IPv6 는 잘 쓰지 않기 떄문에 기본으로 IPv4 만 분할하도록 설정된다. (물론 별도 설정으로 IPv6 도 제어 가능)
* 자동으로 생성되는 VPC (default) 끼리는, VPC 피어링을 할 수 없다.
* 그도 그럴것이, 자동 생성 VPC 는 각 리전별로 미리 생성된 subnet 을 가지고 있기에, VPC 피어링을 하게 되면 subnet 충돌이 발생할게 될 것임
* 이유는 잘 모르겠지만 CIDR 표기에서 마지막 range 설정이 `/29` 까지만 가능하고 그 이상은 불가능하다.

## Firewall
* VPC 단위에서 방화벽 설정을 할 수 있다.
* 다만 VPC 레벨에서 트래픽을 막아주는건 아니고, 실제 VM 에 패킷이 도착해야지만 방화벽 로직이 수행되면서 트레픽 제어를 한다.
  * 아마 VM 자체 또는 그 앞단에서 iptables 같은 걸로 제어하는거 같음.
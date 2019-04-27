# AWS submit Seoul 2019

### 목차/자료
1. 기술트랙 목차: https://aws.amazon.com/ko/events/summits/seoul/agenda/day2/  
2. 슬라이드(ppt형식) 상세내용 확인: https://www.slideshare.net/awskorea  

<hr>

### 내가 들었던 세션중 일부(나에게 의미 있는 부분)

1. 천만 사용자를 위한 웹서비스 구축  
 : MSA구축 및 재기동시, **큐잉패턴** 활용가능(잠시동안 대기 서버 살아날때 큐에서 pop하여 요청내역 재전달)
<br>
 
2. 딥러닝 모델의 효과적인 분산트레이닝 모델 최적화 방법
- AWS 딥러닝 플랫폼  
  : 8개 gpu + 32gbyte  
- tensor core > 16float 변환사용 (학습성능 2~3배 향상가능)  
  : 32>16 변환작업 코딩에 필요
- **horovod**
  : 딥러닝 플랫폼 분산학습지원, 학습시간단축!, 오픈소스
- hyper parameter 최적처리 계산
  : **aws sagemaker HPO**
- AWS NEO
  : 배포최적화를 위한 컴파일러 제공?
- AWS ML 사이트
  : ml.aws.com
 <br>
   
 3. AWS MSA
  - 쿠버네티스 external DNS 기능 제공
  - 서킷브레이커 : 사이드카 방식 프록시 처리 기능지원
  - AWS App MESH: plain controller..

# Google Cloud Next'19 Extended Korea

### 목차/자료
1. 기술트랙 목차: https://festa.io/events/256  
2. facebook 그룹: https://www.facebook.com/groups/googlecloudkorea/

<hr>

### 내가 들었던 세션 요약:

#### 트랙1_1. beyond kubernetes ( Istio, spinnaker, knative,kubeflow ) - 조대협 ( Google )    
 - MSA
 1) MSA : 조직문화 선행되어야 함(기획,디자인,개발,운영,인프라 하나의 팀으로 구성)  
 2) MSA가 맞지 않는 프로젝트: SI(개발후 개발자가 떠남), 대형 Enterprise Proj
 3) MSA 패턴(**MEAP** 책 참고)  
  : MSA -> Circuit Breaker(장애확산 차단기, app 레벨) -> **Envoy Proxy** (인프라 레벨)  
    -> Istio (라우팅 특화, 통신간 서비스 암호화, 가시화 모니터링 tool 제공 등)  
 4) DevOps 팀: Self service Platform 개발을 하는 경우가 많음?  
 - 쿠버네티스  
 1) Pods: 컨테이너간 Data공유, Side-Car패턴(Istio)
 2) 피닉스 서버 배포(Immutable) -> 스핀네이커(Pipeline구성)    
 3) 모니터링 : 구글 **StackDriver** 출시  
  > StackDriver logging 기능 우수: 프로메테네우스 내장? ELK에서 구현하는 복잡함등을 해결하는 대안으로 좋을듯..  
  > StackDriver: Dependency관리(가시화 추적도구 제공), err관리, 추적등 다양한 대시보드 제공(집킨등을 직접 설치 사용하는건 설정할것들이 넘 많음?)   
 - **Knative**  
 1) 현재 1.0 버전까지 올라오진 못함(빌드, 배포까지는 쓸만함, Event는 아직 사용하면 안됨)  
 2) Kubeflow: 머신러닝 엔진  
 3) Anthos : On-Premiss환경에 구글 쿠버네티스 설치 및 사용(VPN을 열어야하는등 장단점있음, 쿠버네티스 관리를 구글에서..)   
 - 발표자료공유(없음), 관련자료
  > 스프링부트&집킨: https://bcho.tistory.com/1319?fbclid=IwAR2S3vo8OervzYGCjuWrpn_NQSjiXVr9fT6H24FtUTJUZBf9DVUTLGGG9wM  
  > 스택드라이버: https://bcho.tistory.com/1321?fbclid=IwAR1HZiF03DVLDAzrv6tgCZcetdZWpBqvMKnfNEyQxTB37N95ggaS3TLKZPc  
  > Knative Serving: https://bcho.tistory.com/1322?fbclid=IwAR0wdB3wdB3Xdg0BB_rHF32M-UGQE39FB-INOVeLT3JPhRuHVQ53afe0zGQ  
  >  
<br>

#### 트랙1_2. Knative로 서버리스 워크로드 구현 - 김진웅 ( SK주식회사 C&C )  
 - Serverless, Paas  
 1)Serverless 플랫폼 -> Knative  
 2)Pass : Function As a Service(AWS lamda등)  
 - **Knative**  
 1) 구성: Build(Source->Image), Serving(배포?), Event  
 2) Cold Start문제(0.5~8초 사이 기동시간 존재): 해결방안-5분마다 한번씩 서비 호출하여 service 항상 On?모드로 있도록 함  
 3) Cloud Run On GKE: Seamless Knative  
 4) Knative Use-Case: https://github.com/steren  
 - 발표자료/github  
 > 김지웅님 github: https://github.com/ddiiwoong  
 > 발표 슬라이드: https://www.slideshare.net/JinwoongKim8/knative?fbclid=IwAR0neUraM7qp9Jf81G-6yAv9Gn6AiaLk5dh4QYK615ULluzMtEl7_d2iqkY  
<br>


#### **트랙2_3. 효율적으로 ML model을 서비스화 해보자 - 오지연 ( 넷마블 )** 
 > ML기반 버그유저? 탐지서비스 GCP기반 상용화 사례 발표 
 1) Idea: **Auto-Encoder**를 이용하여, 정상 사용자는 in/out이 거의 유사, 비정상 사용자는 in/out이 차이가 큰점을 활용  
 2) ML 모델 문제/개선  
  > 버그유저 data 희소성 문제: (해결) 전체data를 학습통해 해결?  
  > 복잡한 모델 + 시계열정보 추가(3차원) : **LSTM**적용, **Stacked Denoising** 적용추가  
 3) GCP 활용
  > GCP 기반 모델 서비스를 프레임워크화 함: 게임이 추가되더라도 프레임워크단에서 일부만 커스터마이징하여 손쉽게 확장가능하게 함.  
  > GCP의 Auto 스케일링(Data사이즈가 크더라도 자동으로 확장되어 학습시간 거의 유사, 분산처리(Node, 머신선택 가능) 기능 활용  
  > GCP의 다양한 옵션 선택사용 가능  
 4) 학습 및 Prediction
  > 학습: Data BigQuery-> 전처리 -> CSV파일 -> Training모델 -> 학습 -> GCS?(파일저장)  
  > 예측: ~~ 예측한 CSV파일 생성 -> 메일전송(전달)
  > 재학습 주기: 1일~1시간등 다양
 5) GCP 활용
  > Model Serving(Online, Batch Prediction)
<br>


#### 트랙1_4. Embulk와 GCP Bigquery를 통한 ETL 맛보기 - 김종건 ( GDG Cloud Korea Organizer )  
 1) 대용량/멀티포맷 파일 로더, 추측(파일분석) 기능 제공  
 2) 파일->(in)->Embuld->(out)->GCP로 DB로딩, GCP->Embulk->타 DB로 전환도 가능  
 > 발표자료 : https://drive.google.com/file/d/1TU-kloCMBImbawpHqVu1iY8HaeAVy9Ds/view?fbclid=IwAR0-Ti4WQqtnnT8KTbcap2fE0N9a2NuAOHwtkCvV6ZmRoJMWnbY3oNnMw6k  
<br>


#### 트랙1_5. Kubernetes와 Docker를 활용한 BaaS 구성기 - 김선종 ( 아티프렌즈 )  
1) Baas: 블록체인 as a Service  
2) 시행착오: Docker compose와 쿠버네티스 compose의 구성이 다름(쿠버네티스용으로 변환과정 필요)  
3) MiniKube로 로컬 설치 및 테스트진행  
> 발표자료: https://docs.google.com/presentation/d/10jYewv5mRoKbfgmSCYTtdxuZmDkniCcwtq5vq8NMF6c/edit?fbclid=IwAR1hTzxiKLVCsdtJT8iqQu3HTKXO6TH_rD_t_FqDkiZDqmPLZhJfxhVajWE#slide=id.p  
<br>


#### **트랙1_6. BQ ML 캐슬 - 이용운**  
1) **BigQuery ML** : GCP기반에서 쿼리Language로 머신러닝 모델학습 및 Prediction 가능  
2) 2018.07 론칭? 및 2019년에 K-means모델 추가됨  
3) 구글 테스트 data제공: Google Analysis Sample Data (2016/08/01~2017/08/01)  
4) 시연  
 4-1) 구매예측  
 4-2) 유저 행동패턴 그룹핑  
 > 발표자료: https://docs.google.com/presentation/d/10jYewv5mRoKbfgmSCYTtdxuZmDkniCcwtq5vq8NMF6c/edit?fbclid=IwAR1hTzxiKLVCsdtJT8iqQu3HTKXO6TH_rD_t_FqDkiZDqmPLZhJfxhVajWE#slide=id.p
<br>

#### 트랙_기타. 좌충우돌 CLOUD 학습기(이동민)
 - Google Cloud Certified Associate Cloud Engineer 취득기(8주학습)  
 > 직접듣지는 못함  
 > 슬라이드 공유: https://www.slideshare.net/DONGMINLEE15/cloud-137939221?next_slideshow=1  
<br>
<br>

### 다음 행사 안내  
- https://festa.io/events/272?fbclid=IwAR2N_Rdq3JuZb-kXO2s4Q5HztYMWZQpcZpzWehGC31EK534cXVmO-98_zec  
 > 관심세션: 쿠버네티스 On-premise 인프라 운영기   
 > 후기공유: https://blog.naver.com/PostView.nhn?blogId=alice_k106&logNo=221528338714&redirect=Dlog&widgetTypeCall=true&directAccess=false    
 

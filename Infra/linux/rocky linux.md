## /etc/yum.repos.d/

1. Rocky-BaseOS.repo
- rocky linux 의 기본 운영체제 구성
- 필수적인 시스템 소프트웨어, 커널, 기본 도구 등과 같은 운영 체제의 핵심 패키지를 포함

2. Rocky-AppStream.repo
- 응용 프로그램과 개발 도구를 제공
- 다양한 버전의 소프트웨어 모듈(Streams) 및 패키지를 포함

3. Rocky-Extras.repo
- 기본 저장소에는 없는 추가적인 패키지를 제공
- 타사 드라이버, 외부 모듈, 특수 패키지 등이 포함


### yum update
- 활성화(enabled=1) 된 저장소에서 업데이트 가능한 패키지 확인
- 각 패키지에 대해 지정된 baseurl 경로에서 최신버전 다운로드하고 설치.

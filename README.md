# HW3

1. 스프링부트로 개발 환경 설정하기
  - Pom.xml에 부트 버전은 2.5.4로 해주었습니다.
    * ![image](https://user-images.githubusercontent.com/71567319/130899007-1972c612-9e27-4df5-bb5f-9dce7102a053.png) ![image](https://user-images.githubusercontent.com/71567319/130899011-74884927-cdd3-4cca-a910-58c947ca55c2.png)

------------

2. 통계 API(SW활용현황)을 위한 DB, TABLE생성
  - 기본 requestInfo 정보 & 데이터 추가
    * ![image](https://user-images.githubusercontent.com/71567319/130961467-8884ee51-6d93-43a1-932f-c4bd4c509ec9.png) ![image](https://user-images.githubusercontent.com/71567319/130961480-402960a5-1fc0-47c5-bfc2-847c3ce93b02.png)
  - user 데이터 추가
    * ![image](https://user-images.githubusercontent.com/71567319/130961639-fadb8384-4993-4185-a548-64b4f5e668ad.png)

------------

3. [20년도 로그인 수 API] 스프링 부트, mybatis, mariadb연동
  - MybatusConfig.java
    * ![image](https://user-images.githubusercontent.com/71567319/130964015-1bd0ccc7-f143-424b-b4f1-0ff55608086f.png)

  - statisticMapper.xml
    * ![image](https://user-images.githubusercontent.com/71567319/130965202-b3696253-754a-4e16-9987-e007d1d1d126.png)

  - StatisticServiceImpl.java
    * ![image](https://user-images.githubusercontent.com/71567319/130964792-887f0902-c413-40f2-933a-c95981217f9e.png) ![image](https://user-images.githubusercontent.com/71567319/130965604-ea59cb0e-bb85-441b-9f7a-82844c9fc768.png) ![image](https://user-images.githubusercontent.com/71567319/130964814-7e571deb-c08b-4c69-b503-0892f4b6d47d.png)

  - settingTest.java
    * ![image](https://user-images.githubusercontent.com/71567319/130965801-bc34c18c-94e3-41f1-bd22-7aff7a24ee06.png)

------------

4. SW활용 현황 통계 API구축을 위한 SQL작성
  - 연도별 접속자 수
    * ![image](https://user-images.githubusercontent.com/71567319/130962989-5a892f26-0de6-4a93-a4b8-3c460e4ab356.png)

  - 월별 접속자 수
    * ![image](https://user-images.githubusercontent.com/71567319/130962999-9e17b285-e248-4eb8-b9cb-70e1bde4efd8.png)

  - 일자별 접속자 수
    * ![image](https://user-images.githubusercontent.com/71567319/130963013-640d9290-9b67-495e-8a1c-4026e568cf78.png)

  - 평균 하루 로그인 수
    * ![image](https://user-images.githubusercontent.com/71567319/130963037-2910844b-7740-479b-9117-475a9952ce97.png)

  - 부서별 월별 로그인 수
    * ![image](https://user-images.githubusercontent.com/71567319/130963065-6245473c-4510-44ab-aad6-8a632f9479f4.png)

  - 휴일을 제외한 로그인 수 (API구축시 추가 구현 필요, 별도의 db생성 또는 공공API활용)
    * 4차 과제에서 진행

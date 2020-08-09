# 일정 계획

* 일정 계획 : 개발 프로세스를 이루는 소작업을 파악하고 순서와 일정을 정하는 작업
    * 개발 모형 결정
    * 소작업, 산출물, 이정표 설정

* 작업 순서
    1. 작업 분해(work breakdown)
    2. CPM 네트워크 작성
    3. 최소 소요 기간을 구함
    4. 소요 MM, 기간 산정 하에 CPM 수정
    5. 간트 차트 작성

## 작업 분해(decomposition)

* 작업 분해 : 프로젝트 완성에 필요한 액티비티를 찾아냄

![work breakdown](https://1t1rycb9er64f1pgy2iuseow-wpengine.netdna-ssl.com/wp-content/uploads/2018/04/WBS-construction-repair-works-Large-Image.jpg)

## 작업 순서 결정 및 소요 시간 예측

* 소작업 / 선행 작업 / 소요 기간을 정리

## 임계 경로

![Critical Path Method](https://t4.daumcdn.net/thumb/R720x0.fpng/?fname=http://t1.daumcdn.net/brunch/service/user/YhJ/image/zaWY7ty_-iJyvPzf8j7DiSGsN7o.png)

* 가장 짧은 일정을 구하는 것이 아니라 가장 긴 일정을 찾고 대략 그 정도의 일정이 걸릴 것이라고 예측하는 것이 합리적

### CPM 네트워크

* 장점
    1. 관리자의 일정 계획 수립에 도움
    2. 프로젝트 안에 포함된 작업 사이의 관계
    3. 병행 작업 계획
    4. 일정 시뮬레이션
    5. 일정 점검, 관리

* 관리에 대한 작업도 포함 가능
* 작업 시간을 정확히 예측할 필요가 있음
* MS-Project 등의 소프트웨어가 있음

## 프로젝트 일정표

* 간트 차트
    1. 소작업별로 작업의 시작과 끝을 나타낸 그래프
    2. 예비시간을 보여줌
    3. 계획 대비 진척도를 표시
    4. 개인별 일정표

## 노력 추정

* 소프트웨어 개발 지용 예측
    * 정확한 비용 예측은 매우 어려움
        * 알려지지 않은 요소가 산재
        * 원가 계산 어려움
    * 과거의 데이터가 필요함
    * 단계적 비용 산정 방법도 사용

* 예산
    * 인건비 : MM 기초
    * 경비 : 여비, 인쇄비, 재료비, 회의비, 공공요금
    * 간접 경비 : overhead

## 비용에 영향을 주는 요소

* 제품의 크기
* 제품의 복잡도
* 프로그래머의 자질
* 요구되는 신뢰도 수준
* 기술 수준(개발 장비, 도구, 조직 능력, 관리, 방법론)
* 남은 시간

## 프로젝트 비용을 예측하는 방법

* 상향식
    * 소요 기간을 정하고 여기에 투입되어야 할 인력과 투입 인력의 참여도를 곱하여 최종 인건 비용을 계산
    * 소작업에 대한 농력을 하나하나 예측
    * 특정한 모델이 없음

* 하향식
    * 프로그램의 규모를 예측하고 과거 경험을 바탕으로 예측한 규모에 대한 소요 인력과 기간을 추정
    * LOC, 기능 점수 모델 사용

## COCOMO 모형

* Barry Boehm 개발
* 하향식 기법
* 회귀분석 사용

![COCOMO 공식](https://slidesplayer.org/slide/15332487/92/images/27/COCOMO+방법+Boehm이+개발+표준+산정+공식+예+CAD+시스템+예상+규모%3A+LOC.jpg)
![COCOMO 비용 예측](https://image3.slideserve.com/6213926/cocomo1-l.jpg)

* KDSI : Kilo Delivered Source Instruction(프로젝트 라인)
* PM : 월 인원(Project Man-month)

### 기본 COCOMO

* 추정된 LOC를 프로그램 크기의 함수로 표현해서 소프트웨어 개발 노력과 비용을 계산

### 중간급 COCOMO

* 프로그램 크기의 함수와 제품, 하드웨어, 인적 요소, 프로젝트 속성들의 주관적인 평가를 포함하는 비용 유도자(cost driver)의 집합으로 소프트웨어 개발 노력을 계산

![COCOMO table](https://lh3.googleusercontent.com/proxy/132XisnZCMGdZuUIgzJd_j9PVUw-9IXVr4vab6hbWOmXBPPIZ0gTzafk8po829bdQyXl_4LH48CLk_pNYIUFoddC6jp_bYkKLL2A7IZEtC8lHfu144E7jfbqJQvxjOiT4raif01YmHAk8X3bJbBLGhTEJHMCp5A)

* 계산한 MM에 모든 노력 승수를 곱해서 계산

* 단점
    * 소프트웨어 제품을 하나의 개체로 보고 승수들을 전체적으로 적용시킴
    * 실제 대부분의 대형 시스템은 서로 상이한 서브시스템으로 구성된 상태에서 각 서브시스템의 성격이 다름

### 고급 COCOMO

* 소프트웨어 공학 과정의 각 단계(분석, 설계 등)에 비용 유도자의 영향에 관한 평가를 도입, 중간급 모형의 모든 특성을 통합시킨 것
* 시스템을 서브시스템으로 분해한 후 각 서브시스템마다 중간급 COCOMO 적용

## COCOMO2

* 1995년 발표
* 소프트웨어 개발 프로젝트가 진행된 정도에 따라 3가지 다른 모델 제시

1. 1단계 : 프로토타입 만드는 단계
2. 2단계 : 초기 설계 단계
3. 3단계 : 구조 설계 이후 단계

![COCOMO2 단계별 설명1](https://blog.kakaocdn.net/dn/b9Y0Se/btqDtAwEOFK/HUU7W2V8BGivutzjF4piIk/img.png)
![COCOMO2 단계별 설명2](https://blog.kakaocdn.net/dn/7wTtT/btqDp2ukRF4/d6rmK6gqBaKKVKYz1iqPU1/img.png)
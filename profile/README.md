# [공공데이터 API를 이용한 노인 일자리 분석](https://github.com/DW-BI-Project)
- 프로젝트 기간
  - (1차) 2023.05.29 ~ 2023.06.02  -  [1차 프로젝트 링크](https://github.com/SilverWork/silverwork_legacy)
  - (2차) 2023.06.26 ~ 2023.06.30
<br> 

## 1. Project Outline
 노인일자리 사업은 65세 이상 노인들에게 경제적 자립과 사회 참여를 목적으로 정부에서 시행하고 있는 복지사업입니다. 
최근 3개년 동안의 노인일자리 공고와 사업정보를 수집하여 노인일자리 채용 현황과 고용 지속성을 조사합니다. 

<br> 

 노인일자리 사업은 경제적 자립과 사회 참여를 촉진하는데 중요한 역할을 합니다. 그러나 사업의 실제 현황과 채용 동향에 대한 정확한 정보 부족으로 인해 정책의 효과성을 평가하기 어렵습니다. 
 이 프로젝트는 노인일자리의 공고와 사업 정보를 분석하여 노인일자리 구인 동향을 파악하고, 정부의 노인 복지 정책의 효과성을 평가할 수 있도록 대시보드를 구성하였습니다.
또한, 노인 일자리 관련 데이터를 수집하고 적재하여 대시보드에 보여지기까지의 과정을 자동화 하였습니다.

<br> 

> __기대효과__
> - 노인일자리에 대한 상세한 이해를 도모할 수 있습니다.
> - 노인일자리 정책의 성과와 한계를 파악하고 개선할 수 있는 방안을 모색할 수 있습니다. 



<br> 



## 2. Team Members & Roles
| 이름 | 역할 | Github |
|:---:|:---:|:---:|
|**남윤아**|**Airflow를 이용한 ETL 구축**|**[@namuna309](https://github.com/namuna309)**
|**김상희**|**대시보드 구축 및 고도화**|**[@ksh1357](https://github.com/ksh1357)**
|**김혜민**|**GCP 인프라 구축**|**[@HyeM207](https://github.com/HyeM207)**
|**이하윤**|**GCP 인프라 구축**|**[@ha6oon](https://github.com/ha6oon)**
|**이성희**|**DBT를 이용한 ELT 구축**|**[@gracia10](https://github.com/gracia10)**

<br> 


## 3. Tech Stack
| Field | Stack |
|:---:|:---|
| 사용 언어 | <img src="https://img.shields.io/badge/python-3776AB?style=flat&logo=python&logoColor=white"/> |
| 데이터 적재 | <img src="https://img.shields.io/badge/bigquery-4285F4?style=flat&logo=googlecloud&logoColor=white"/> <img src="https://img.shields.io/badge/Google Cloud Storage-4285F4?style=flat&logo=googlecloud&logoColor=white"/> <img src="https://img.shields.io/badge/google sheets-34A853?style=flat&logo=googlesheets&logoColor=white"/> |
| ETL & ELT | <img src="https://img.shields.io/badge/airflow-017CEE?style=flat&logo=apacheairflow&logoColor=white"/> <img src="https://img.shields.io/badge/DBT-FF694B?style=flat&logo=dbt&logoColor=white"/> |
| infra | <img src="https://img.shields.io/badge/Google Cloud Platform-4285F4?style=flat&logo=googlecloud&logoColor=white"/> |
| CI/CD | <img src="https://img.shields.io/badge/Git Action-2088FF?style=flat&logo=githubactions&logoColor=white"/> |
| Dashboard | <img src="https://img.shields.io/badge/Superset-00D1B2?style=flat"/> |
| 협업 도구 | <img src="https://img.shields.io/badge/github-181717?style=flat&logo=github&logoColor=white"/> <img src="https://img.shields.io/badge/slack-4A154B?style=flat&logo=slack&logoColor=white"/> <img src="https://img.shields.io/badge/notion-000000?style=flat&logo=notion&logoColor=white"/>|

<br> 

## 4. 인프라
### 4 - 1. 인프라 구조
![Untitled](https://github.com/DW-BI-Project/silverwork-airflow/assets/68600766/23971e0f-abd4-4392-b0f3-8489dcd23c7d)
- **Google Kubernetes Engine:**
    - **Airflow** : 클라우드 기반 Kubernetes 서비스인 **GKE** 위에 Airflow가 동작함
- **Github** : DAG 저장소
- **Google Sheets:** 공공데이터 API 데이터가 1차 적재
- **Google Cloud Storage** : Google Sheets의 데이터가 Cloud Storage에 2차 적재
- **BigQuery** : Raw data와 Analytics, Adhoc 스키마 저장소
- **Preset** : Bigquery에서 데이터를 가져와 시각화
### 4 - 2. GKE 내 Airflow 구축
- GKE 내 Kubernetes 클러스터 생성
![Untitled (5)](https://github.com/DW-BI-Project/silverwork-airflow/assets/68600766/7cb8652e-86c5-405f-abc4-0aad53069efa)
- 공식 Apache Airflow Helm 차트 배포(GKE에 Airflow 배포)
![Untitled (9)](https://github.com/DW-BI-Project/silverwork-airflow/assets/68600766/28742a1c-20a7-4048-b00c-05ff97ecce1d)
- values.yaml 수정 후 재배포
  
  ![Untitled (7)](https://github.com/DW-BI-Project/silverwork-airflow/assets/68600766/65fb78ef-7404-4921-acc7-21fdaf3c2684)
<br>

## 5. Data flow
### 5 - 1. Airflow(ETL)
![그림1](https://github.com/DW-BI-Project/silverwork-airflow/assets/68600766/be98b8d9-9596-4aed-ba52-65486a31231a)
- 공공데이터 포털, Vworld 등 api를 통해 데이터를 추출 후 전처리 과정을 거쳐 최종적으로 Bigquery 에서 raw dataset 밑에 jobs테이블과 projects 테이블에 적재
- Airflow를 활용하여 구인 공구 정보는 UTC기준 매일 자정 jobs_crawling DAG 실행, 사업 정보는 동작 시 한번만 projects_crawling DAG 실행 
### 5 - 2. DBT(ELT)
- DBT 프로세스
![그림2](https://github.com/DW-BI-Project/silverwork-airflow/assets/68600766/bb788021-73f2-45f5-a934-76aa30151046)
    - DBT를 이용해 DW의 raw 데이터를 검증하고 분석에 적합한 데이터로 가공
    - DBT 이미지를 생성하여 Airflow를 통해 스케줄링

- DBT 배포 프로세스 구현
![그림3](https://github.com/DW-BI-Project/silverwork-airflow/assets/68600766/03af6947-a701-4ebb-bb8f-5d77a349a1c7)
    - Airflow 와 코드베이스를 분리하기 위해 DBT 프로젝트를 별로도 생성
    - devleop , main 브랜치에 PR 이벤트가 발생하면 dev, stage dw에 모든 모델을 생성하고 검증
    - main 브랜치에 push 이벤트가 발생하면 도커이미지를 생성하고 Airflow DAG를 트리거

<br>

## 6. Dashboard
### 6 - 1. 채용 공고
![Untitled (2)](https://github.com/DW-BI-Project/silverwork-airflow/assets/68600766/cbde3ae3-aa8d-4169-b398-2e6f830f8817)
- Dimension을 적용하여 지역 및 년도 별로 데이터 확인 가능
### 6 - 2. 고용 지속성
![Untitled (3)](https://github.com/DW-BI-Project/silverwork-airflow/assets/68600766/acfafdc2-fe76-4bfa-8a2e-85b4666e81aa)
- 사용자의 차트 가시성 확보를 위해 title, label, legend 등 추가
- 막대 그래프에 Zoom 기능 추가로 원하는 부분 확대 가능
### 6 - 3. 일자리 공고
![Untitled (4)](https://github.com/DW-BI-Project/silverwork-airflow/assets/68600766/95411577-ce85-4d14-8c09-2f7e423d50d2)
- 일부 차트의 유형 변경
- 우대 사항 차트의 경우, 우대 사항 정보가 개수보다 중요하다고 판단하여 Pie Chart에서 WordCloud로 변경


## 7. Dashboard 시연
![노인 일자리](https://github.com/learn-programmers/KDT_DATA_1st/assets/62321695/741826fd-f77c-436e-ae6f-a3bbb781b6bf)



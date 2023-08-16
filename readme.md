![Footer__1_](./images/masida_logo.png)

SSAFY 8기 2학기 특화 프로젝트
2023.02.20 ~ 2023.04.07

<br>

# 1. MASIDA 소개

### **당신의 취향을 그대로 담은 칵테일을 집에서도 손쉽게🍸**

🌵 Naming : 마시다와 맛있다의 중의적인 표현

| 늘어나는 혼술족과 칵테일을 다양하게 시도해보고 싶은 사용자를 위한 서비스,

| 칵테일 레시피를 찾고, 자신의 취향에 맞는 칵테일을 추천, 분석해주는 서비스.

<br>

# 2. 🔍 개발 환경

## 2-1. 환경 설정

### **👨‍💻 Front-end**

    - React 18.2.0

    - Node 18.12.1

    - npm 8.19.2

    - Next 13.2.3

### **👨‍💻 Back-end**

    - Java 11

    - SpringBoot 2.7.9

    - Python 3.9

    - MySQL 8.0

    - Fast API

    ※ [설치 파일](./pythonProject/requirements.txt/)

### **👩‍💻 CI/CD**

    - AWS EC2

    - Jenkins

    - Docker 20.10.18


## 2-2. 서비스 아키텍처

![Architecture](./images/masida_architecture.png)

<br>

# 3. 📘 실행 방법

## docker를 활용한 실행 가이드

1. **git clone**

```bash
git clone
```

2. **[도커 설치](https://docs.docker.com/get-docker/) 

3. **Dockerfile 및 nginx 설정파일 작성**

   - nginxec2.conf 파일

```bash
  server {
    
      location /{
          proxy_pass http://localhost:3000;
      }

      location /api {
          proxy_pass http://localhost:8080/api;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection "upgrade";
          proxy_set_header Host $http_host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      }

      listen 443 ssl;
      ssl_certificate ${fullchain.pem}
      ssl_certificate_key ${privkey.pem}

  }

  server {
      if ($host = ${server_name}) {
          return 301 https://$host$request_uri;
      } # managed by Certbot

          listen 80;
          server_name ${server_name};
      return 404; # managed by Certbot
  }
     ```

4. **도커 컨테이너 실행**

   - mysql 이미지 실행하기

     ```bash
      # mysql 이미지 가져오기
      docker pull mysql
      # 컨테이너 실행
      docker run --name mysql -e MYSQL_ROOT_PASSWORD={password} -d -p 3306:3306 mysql
     ```


5. **작동 확인**

- 실행 중인 컨테이너 조회

  ```bash
  docker ps
  ```

- mySQL 접속하여 DB [덤프 파일](/exec/b208_masida_dumps.sql) 실행

<br>

# 4. 🦈 주요 기능



### 1. 메인 배너
   
![메인 배너](./images/image_8.PNG)   
-  메인 페이지의 배너를 통해 입문자, 봄, 여름, 칵테일의 재밌는 정보를 볼수 있습니다.

<br>

### 2. 칵테일 검색
   
![검색 페이지](./images/searchpage.PNG)   
- 검색 페이지에서 이름, 베이스, 색상, 난이도, 재료별로 검색이 가능하며 그 결과에 따라 처음엔 이름순으로 보여주며 좋아요, 별점순으로 나열이 가능합니다.

<br>

### 3. 칵테일 상세 페이지
     
![상세 페이지](./images/detail.PNG)   
- 상세페이지에서는 이 칵테일의 정보들에 대해서 볼수 있으며 정보는 별점과 좋아요, 댓글의 개수, 난이도, 재료, 레시피, 소개 정보 등이 왼쪽에 보여지며 오른쪽에는 상세페이지의 칵테일과 비슷한 재료로 만든, 색인 칵테일을 보여줍니다.
- 로그인시 오른쪽의 버튼을 클릭하면 댓글 창을 띄워서 보여줍니다.

### 3-1. 댓글 모달   

![댓글 모달](./images/detailcomment.PNG)   
- 다른 사람이 해당 칵테일에 대한 댓글을 볼 수 있으며 내가 단 댓글은 수정 및 삭제가 가능합니다. 해당 별점, 난이도를 매김에 따라 칵테일의 별점, 난이도가 변하며 댓글은 인당 1개씩만 작성 가능합니다.

<br>

### 4. 마이 페이지
   
![마이 페이지](./images/mypage.PNG)   
- 로그인을 해야 들어 올 수 있는 구간입니다. 헤더에는 카카오톡 프로필의 이름을 띄워줍니다.
- 페이지에서는 현재 나의 카카오톡 프로필 사진과 북마크, 좋아요 누른 것의 개수를 볼 수 있고 현재 내가 좋아요 누른 칵테일에 대한 분석을 아래에 보여줍니다.
- 오른쪽에는 내가 북마크, 좋아요, 댓글이 있는 칵테일을 볼 수 있습니다. 그 칵테일을 누를시 칵테일 상세보기 페이지로 이동됩니다.

<br>

### 5. 칵테일 월드컵
   
![칵테일 월드컵](./images/worldcup.PNG)   
- 월드컵 페이지로 오면 랜덤 칵테일 16개에 대해 월드컵을 진행하며 해당 칵테일에 마우스를 갖다 대면 카드가 뒤집어지며 해당 칵테일에 대한 설명이 나옵니다.

### 5-1. 칵테일 월드컵(우승 칵테일)   
![칵테일 월드컵 우승](./images/worldcup_winner.PNG)   
- 우승자라는 것을 보여주며 마찬가지로 칵테일에 마우스를 갖다 대면 카드가 뒤집어지며 설명이 나옵니다.
- 이 칵테일의 난이도, 별점, 좋아요, 댓글의 수가 나오며 다시하기 버튼을 클릭시 월드컵을 16강부터 다시하며 상세보기 페이지로 갈수있는 자세히 보기 버튼도 있습니다.
- 아래에는 댓글이 존재한다면 해당 칵테일에 대한 댓글을 보여주며 없을시 댓글이 존재하지 않는다고 나옵니다.

<br>

### 6. 사용자 취향 분석 및 추천

![유저 칵테일 분석](./images/analysis.JPG)   

<br>

# 5. 🔍 추천 알고리즘

- 재료와 색상 기반 유사한 칵테일 추천 (칵테일 상세 페이지)
  - 콘텐츠 기반 필터링
  - BoW, TF-IDF, Cosine Similarity 을 활용하여 구현

- 사용자의 베이스, 재료, 색상의 취향에 따른 칵테일 추천 (사용자 취향 분석 페이지)
  - 콘텐츠 기반 필터링
  - FastAPI, Jaccard Similarity, Python 을 활용하여 구현

<br>

# 6. 🛡 배포

- https
  - certbot 컨테이너를 함께 실행
  - letsencrypt ssl 인증서 발급
  - EC2 도메인 사용하여 인증
- 자동 배포
  - Gitlab에서 web hook 설정을 통해 jenkins 빌드 유발
  - jenkins의 shell script 실행 기능을 이용하여 git pull, docker build 실행

<br>

# 7. 📁 설계 문서

### 7-1. ERD

![ERD](./images/masida_erd.png)

### 7-2. Figma

![Figma](./images/figma.JPG)

<br>

# 8. 🖊 Cooperation&Promises

### 8-1. Tools

    - Git

    - Jira

    - Notion

    - Mattermost

    - Webex



<br>

# 9. 👨‍👩‍👧‍👦 팀원 소개

![Team_Madida](./images/ourTeam.PNG)
- 프론트엔드: 손종효, 김지환, 김영주
- 벡엔드: 이지현(Infra), 강지수, 김주성

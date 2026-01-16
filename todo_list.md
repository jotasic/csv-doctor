# 📅 CSV Doctor Development Roadmap

## 🚀 Phase 0: 프로젝트 초기 세팅 (Setup)

- [ ] GitHub Repository 생성 및 초기화
    - .gitignore (Python, Terraform, MacOS 용) 설정
    - README.md에 기획서 내용 요약 작성

- [ ] 로컬 개발 환경 구성 (Docker)
    - docker-compose.yml 작성 (DynamoDB Local, Dynamodb Admin)
    - requirements.txt 작성 (FastAPI, Mangum, Boto3, Pytest)
    - 가상환경(venv) 설정 및 패키지 설치

## 🧠 Phase 1: 백엔드 핵심 로직 개발 (Backend Core)

- [ ] FastAPI + Mangum 기본 구조 잡기
    - app/main.py: Hello World 엔드포인트 작성
    - 로컬 실행 테스트 (uvicorn app.main:app --reload)

- [ ] CSV 변환 로직 모듈 구현 (csv_fixer.py)
    - cp949, euc-kr 등 인코딩 감지 로직 (chardet 활용)
    - utf-8-sig (BOM) 변환 및 저장 로직
    - 단위 테스트 작성 (tests/test_fixer.py)

- [ ] Presigned URL 발급 API 구현
    - GET /upload-url: 업로드용 URL 생성 (Boto3)
    - GET /status/{task_id}: S3 객체 존재 여부 확인 (Polling용)

## 🏗️ Phase 2: 인프라 코드 작성 (Infrastructure / Terraform)

- [ ] Terraform 기본 설정
    - provider.tf: AWS 프로바이더 및 리전 설정
    - backend.tf: State 파일 관리 설정 (로컬 또는 S3)

- [ ] S3 버킷 리소스 정의
    - 업로드용 버킷 생성
    - Lifecycle Rule: 1일 후 자동 삭제 설정
    - CORS 설정: 프론트엔드(Localhost, Production 도메인) 접근 허용

- [ ] Lambda & Function URL 정의
    - aws_lambda_function: FastAPI 코드 배포 설정
    - aws_lambda_function_url: API Gateway 대신 무료 URL 생성
    - IAM Role: S3 접근(GetObject, PutObject) 및 CloudWatch 로그 권한 부여

- [ ] S3 Event Trigger 연결
    - aws_s3_bucket_notification: 파일 업로드 시 람다 실행 트리거 연결
    - (옵션) 태그 기반 로직 분기를 위한 권한(s3:GetObjectTagging) 추가

## 🎨 Phase 3: 프론트엔드 구현 (Frontend)

- [ ] 기본 UI 스캐폴딩 (Single File)
    - static/index.html 생성
    - Tailwind CSS, Alpine.js CDN 연결
    - Lottie Player 연결

- [ ] 파일 업로드 로직 구현 (JS)
    - Drag & Drop 이벤트 처리
    - fetch로 Presigned URL 요청
    - S3로 파일 PUT 업로드 (헤더에 태그 포함)

- [ ] 상태 확인(Polling) 및 다운로드 구현
    - setInterval로 2초마다 상태 체크 API 호출
    - 완료 시 다운로드 버튼 활성화

- [ ] 예외 처리 및 UX 개선
    - 502/503 에러 시 "서버 점검 중" 모달 띄우기
    - 다국어(i18n) JSON 데이터 적용

## 🤖 Phase 4: 자동화 및 배포 (CI/CD)

- [ ] GitHub Actions 워크플로우 작성
    - .github/workflows/deploy.yml
    - pytest 실행 (실패 시 배포 중단)

- [ ] Terraform 자동 배포 설정
    - GitHub Secrets에 AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY 등록
    - terraform apply -auto-approve 단계 추가

- [ ] E2E 테스트 (선택 사항)
    - Playwright로 프론트엔드 업로드 시나리오 자동화 테스트

## 📢 Phase 5: 출시 및 마케팅 (Launch)

- [ ] CloudFront 도메인 연결
    - api.csvdoctor.com -> Lambda Function URL 연결
    - www.csvdoctor.com -> S3 정적 호스팅 연결

- [ ] 검색 엔진 최적화 (SEO)
    - meta 태그 (OG Title, Description) 작성
    - sitemap.xml, robots.txt 생성 및 제출

- [ ] 광고 및 분석 도구 부착
    - Google Analytics 4 (GA4) 스크립트 삽입
    - 개인정보처리방침 페이지 생성
    - (콘텐츠 보강 후) 구글 애드센스 신청
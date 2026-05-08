## 프로젝트 개요
- **목적** : AI 기반 루틴 관리 서비스 개발 (2025 졸업작품)
- **개발 기간** : 2025.09 ~ 2025.11

## 기술 스택
**Backend**
- Framework: Spring Boot 3.5.5
- Language: Java 21
- Database: PostgreSQL (Spring Data JPA)
- Auth: JWT (JJWT) + Supabase Auth (JWKS 기반 토큰 검증)

**AI Server**
- Google Cloud Vertex AI (Gemini) — Function Calling 기반 챗봇 개발
- Python FastAPI + YOLOv11 — 한식 음식 객체탐지

**Infrastructure**
- Docker, Docker Compose, Nginx (리버스 프록시)

<img width="1692" height="792" alt="Image" src="https://github.com/user-attachments/assets/6b374e72-02ae-43de-bd1c-61692aa547d8" />

## 주요 구현 내역
- AI 챗봇 기능 개발 (Function Calling을 사용하여 DB를 직접 조작)
- 음식 이미지 탐지 서버 + 모델 개발 (YOLOv11 모델을 사용하여 한식 이미지 인식, Spring서버가 Proxy로 중계)
- 그 외 여러가지 API 개발 (CRUD)

## 트러블슈팅
**1) AI 응답에 내부 함수 이름 노출**
- **문제** : Vertex AI Function Calling 사용 시 AI가 응답 텍스트에 내부 함수명(예: `check_routine`)을 그대로 포함시켜 사용자에게 노출시키는 문제가 있었음
- **해결** : 시스템 프롬프트에 해당 지침을 추가하는 방식으로 해결

**2) AI가 루틴 ID를 불필요하게 재확인하는 문제**
- **문제**: AI가 DB를 조작하려 할 때, 이미 컨텍스트에 있는 ID값을 굳이 재확인하는 불필요한 단계를 반복하여 동작이 느려지는 문제가 있었음
- **해결**: Function의 설명을 구체화하고, 사용자의 데이터를 시스템 프롬프트에 사전주입하는 방식으로 해결

**3) Docker 이미지 오류**
- **문제** : Google 인스턴스 내에서 코드 업데이트 중, 기존 이미지가 더 이상 정상적으로 동작하지 않아 컨테이너가 뜨지 않는 문제 발생
- **해결** : temurin으로 변경하였음

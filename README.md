# RunArchive 🏃

러너들을 위한 개인 러닝 기록 아카이브 웹앱입니다. 대회 기록, 특별한 장소에서의 달리기, 해외 여행지에서의 러닝 등 소중한 순간들을 기록하고 관리할 수 있습니다.

## 주요 기능

### ✅ MVP (Phase 1)
- 🔐 **이메일 인증** - Firebase Authentication을 통한 안전한 로그인
- 👤 **프로필 관리** - 프로필 사진, 이름, 자기소개
- 📝 **러닝 기록 관리**
  - 날짜, 거리, 시간, 페이스 자동 계산
  - 장소 기록 (특별한 장소 기억하기)
  - 사진 첨부
  - 감정/메모 작성
  - 공개/비공개 설정
- 🏆 **개인 최고 기록** - 5K, 10K, 하프, 풀마라톤 자동 인식 및 PR 표시
- 📱 **인스타그램 스타일 UI** - 피드 형식으로 기록 확인

### 🔜 향후 개발 예정 (Phase 2)
- 🗺️ 지도 연동 (Google Maps API)
- 📄 대회 기록증 업로드
- 👥 소셜 기능 (팔로우, 좋아요, 댓글)

## 기술 스택

- **Frontend**: React 18, Tailwind CSS
- **Backend**: Firebase
  - Authentication (이메일 인증)
  - Firestore (NoSQL 데이터베이스)
  - Storage (이미지 저장)
- **Hosting**: Netlify
- **Version Control**: GitHub

## 설치 및 배포 가이드

### 1단계: Firebase 설정 (완료됨 ✅)

Firebase 프로젝트가 이미 설정되어 있고, 설정 정보가 코드에 포함되어 있습니다.

**확인 사항:**
- ✅ Authentication (이메일/비밀번호) 활성화
- ✅ Firestore Database 생성
- ✅ Storage 활성화 (Blaze 플랜)
- ✅ 보안 규칙 적용

### 2단계: GitHub 저장소 생성 및 푸시

#### 2-1. GitHub 저장소 만들기

1. https://github.com 로그인
2. 오른쪽 상단 "+" → "New repository" 클릭
3. Repository name: `runarchive` 입력
4. Public 선택
5. **"Add a README file" 체크 안 함**
6. "Create repository" 클릭

#### 2-2. 로컬에서 Git 설정

다운로드한 파일들이 있는 폴더에서 터미널/명령 프롬프트를 열고:

```bash
# Git 초기화
git init

# 모든 파일 추가
git add .

# 첫 커밋
git commit -m "Initial commit: RunArchive with Firebase integration"

# 메인 브랜치로 변경
git branch -M main

# GitHub 저장소 연결 (본인의 username으로 변경!)
git remote add origin https://github.com/YOUR_USERNAME/runarchive.git

# 푸시
git push -u origin main
```

**처음 Git을 사용하는 경우:**
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### 3단계: Netlify 배포

#### 3-1. Netlify 계정 생성

1. https://www.netlify.com 접속
2. GitHub 계정으로 가입/로그인

#### 3-2. 사이트 배포

1. 대시보드에서 **"Add new site"** 클릭
2. **"Import an existing project"** 선택
3. **"Deploy with GitHub"** 선택
4. GitHub 계정 인증 및 저장소 접근 권한 부여
5. `runarchive` 저장소 선택
6. **빌드 설정:**
   - Branch to deploy: `main`
   - Build command: (비워두기)
   - Publish directory: `.`
7. **"Deploy site"** 클릭

#### 3-3. 배포 완료

- 몇 초 후 배포 완료!
- 자동 생성된 URL 확인 (예: `https://random-name-123.netlify.app`)
- 원하는 이름으로 변경 가능 (Site settings > Domain management > Edit site name)

### 4단계: 테스트

1. Netlify URL로 접속
2. 회원가입 테스트
3. 프로필 설정
4. 첫 러닝 기록 추가
5. Firebase Console에서 데이터 확인

## 프로젝트 구조

```
runarchive/
├── index.html              # HTML 엔트리 포인트
├── running-archive.jsx     # React 앱 메인 코드 (Firebase 설정 포함)
├── package.json           # 프로젝트 메타데이터
├── netlify.toml           # Netlify 배포 설정
├── firestore.rules        # Firebase 보안 규칙
├── .gitignore            # Git 제외 파일 목록
└── README.md             # 이 파일
```

## 데이터 구조

### Firestore Collections

**runs** (달리기 기록)
```javascript
{
  userId: string,           // 사용자 ID
  date: string,            // 날짜 (YYYY-MM-DD)
  distance: number,        // 거리 (km)
  time: number,            // 시간 (초)
  pace: string,            // 페이스 (분:초/km)
  location: string,        // 장소
  memo: string,            // 메모/감정
  photos: string[],        // 사진 URL 배열
  isPublic: boolean,       // 공개 여부
  raceType: string,        // 5K, 10K, HALF, FULL, CUSTOM
  createdAt: timestamp     // 생성 시간
}
```

## 코드 업데이트 방법

코드를 수정한 후:

```bash
# 변경사항 추가
git add .

# 커밋
git commit -m "설명: 무엇을 변경했는지"

# 푸시
git push
```

푸시하면 Netlify가 자동으로 새 버전을 배포합니다!

## 주요 기능 설명

### 자동 PR (개인 최고 기록) 인식
- 거리가 5km, 10km, 21.0975km (하프), 42.195km (풀)와 ±100m 이내로 일치하면 자동으로 해당 종목으로 분류
- 각 종목별 최고 기록을 자동으로 계산하여 대시보드에 표시

### 페이스 자동 계산
- 거리와 시간을 입력하면 자동으로 km당 페이스(분:초) 계산

### 공개/비공개 설정
- 각 기록마다 공개/비공개 설정 가능
- Phase 2에서 소셜 기능 추가 시 공개 기록만 다른 사용자가 볼 수 있음

## 비용 관리

Firebase Blaze 플랜을 사용하지만, 개인 사용 시 거의 무료:
- Storage: 5GB 저장, 1GB/일 다운로드 무료
- Firestore: 50K 읽기, 20K 쓰기/일 무료
- 예상 월 비용: **0원 ~ 1,000원**

**예산 알림 설정:**
Firebase Console > 결제 > 예산 설정 (1,000원 또는 5,000원)

## 문제 해결

### Firebase 오류
- `firebaseConfig` 정보가 올바른지 확인
- Firebase Console에서 모든 서비스 활성화 확인

### Netlify 배포 오류
- `netlify.toml` 파일 포함 확인
- GitHub 저장소가 Public이거나 Netlify 권한 부여 확인

### 사진 업로드 실패
- Storage 활성화 및 보안 규칙 적용 확인
- 파일 크기 5MB 이하 확인

## 라이선스

MIT License

---

**Made with ❤️ for runners**

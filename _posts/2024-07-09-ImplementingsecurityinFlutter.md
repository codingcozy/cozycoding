---
title: "Flutter에서 보안 구현하는 방법"
description: ""
coverImage: "/uidev-css.github.io/assets/no-image.jpg"
date: 2024-07-09 22:40
ogImage: 
  url: /uidev-css.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Implementing security in Flutter"
link: "https://medium.com/@nooralibutt/implementing-security-in-flutter-7eb5e53be66e"
---


- 인증서 핀 설정
- 루트 탐지
- Jailbreak 탐지
- Touch ID, Face ID 또는 패턴과 같은 생체 인증을 사용한 인증
- 민감한 정보를 키체인 (iOS)이나 키스토어 (Android)에 저장
- 비밀 키를 버전 컨트롤에서 제외
- 공격자가 응용 프로그램을 역공학화하고 API 키를 노출하는 것을 어렵게하기 위해 난독화 사용
- API 키 안전하게 유지: .env 파일에서 API 키 로드
  .env 파일에서 API 키를 가져오려면 다음 패키지를 사용하세요: flutter_dotenv
- 정보 및 저장소 암호화: Flutter는 `dart:crypto` 라이브러리를 통해 암호화를 지원하며 해싱, 대칭 암호화 및 비대칭 암호화와 같은 다양한 암호 함수가 포함되어 있습니다.
- 네트워크 호출: 네트워크 통신은 HTTPS 및 SSL/TLS 프로토콜을 사용하여 데이터 가로채기 및 조작을 방지하고 데이터 무결성을 보장해야 합니다.
- 백그라운드 스냅샷 제어: secure_application 패키지 사용
  사용자가 앱을 나가면 앱 내용 가시성을 보호합니다. 사용자가 돌아왔을 때 앱 전환기에 있는 내용을 숨기고 잠긴 내용 위에 서리 장벽을 표시합니다.
- 제 3자 라이브러리 사용 피하기: Flutter에는 작업을 쉽게하는 많은 패키지가 있지만 가능한 한 제 3자 라이브러리 사용을 피해야 합니다. 패키지 내부에서 무슨 일이 일어나고 있는지 알 수 없기 때문입니다. 신뢰할 수 있는 패키지만 사용하는 것이 좋습니다. 패키지에 많은 기여자가 있고 많은 사람들이 사용하고 있으면 안전하게 사용할 수 있습니다.
- 보안 테스트 수행: 잠재적인 취약점을 식별하고 보안 조치가 효과적인지 확인하기 위해 정기적으로 보안 테스트를 수행해야 합니다. 이는 침투 테스트, 코드 리뷰 및 취약점 검색을 포함할 수 있습니다.
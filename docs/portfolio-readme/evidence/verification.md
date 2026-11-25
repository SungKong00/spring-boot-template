# Spring Boot Template 검토 기록

- 기준: `3132cdf4886b4002a62496e6a7011b71d7d638f3`. 원본을 수정하지 않고 `git archive`로 격리 사본을 만든 뒤 `parcel-service` / `io.example.parcel`로 초기화했다.
- 환경: macOS ARM64, 복제한 Corretto 25 및 Gradle 캐시, 별도 사본의 `scripts/gradlew-java25 --offline --no-daemon spotlessCheck build`.
- 결과: Spotless·build·OpenAPI 기준 파일 비교 통과. Java 기본 테스트 30건 통과, OpenAPI 생성 테스트 1건 통과. Python 초기화 테스트는 43건 중 25건 실행·통과, 18건 건너뜀.
- 산출물: 사본에서 빌드한 `parcel-service.jar`에 명시적으로 `local` 프로필을 지정했다. 랜덤 로컬 포트에 바인딩하고 `lsof`로 리스너가 `127.0.0.1`임을 확인했다. [HTTP 결과 발췌](http.txt)는 실제 요청 응답을 형식만 정리한 기록이다. 실행 프로세스는 종료했다.
- 초기화 직후에는 strict 검사를 통과했다. 사본에서 정상적인 Java 소스 수정 한 건을 만들면 strict 검사는 fingerprint 불일치로 실패하고 `--runtime-ready`는 통과했다. [검사 출력](init.txt)
- 정상 빌드의 실제 HTTP 테스트는 Spring Boot의 random-port context를 검증한다. 이번 별도 프로브는 JAR을 실행했다. 문서의 `scripts/run-local.sh` 전체 경로를 새로 실행했다는 뜻은 아니다.
- 이번에 Docker/MySQL 통합 테스트, GitHub Actions, AWS·EC2·RDS·OIDC는 실행하지 않았다. 운영 자산은 설정·스크립트로 존재하지만 해당 환경에서의 배포 성공으로 해석하지 않는다.

## 실제 화면

- 새로 기동한 동일 JAR의 루프백 Swagger UI에서 정상 입력과 공백 입력을 각각 실행했다. 200과 400이 실제 응답 영역에 나타났으며, 400 응답에서 서버가 생성한 요청 ID가 본문과 응답 헤더에서 일치했다. 캡처는 응답 영역만 잘랐고 UI나 결과는 변경하지 않았다. 브라우저는 루프백 외 요청을 허용하지 않았다.
- 연속 GIF도 같은 JAR의 실제 Swagger 요청을 사용했다. 한 브라우저 세션에서 실제 입력값 편집 → 서버 200 → 공백으로 편집 → 서버 400을 실행하고 400 본문·헤더의 생성 요청 ID 일치를 다시 확인했다. 105개의 연속 화면 프레임을 8fps GIF로 변환하고 화면 테두리만 잘랐다. 요청·응답의 교체나 가짜 UI는 없다.

- README 개정: 동일 JAR에 수령 안내 문구 `Package 4821 is ready for pickup.`를 보냈다. Swagger에서 200과 입력 문자열의 반환을 확인했고, 이어서 공백 요청의 400 및 본문·헤더의 요청 ID 일치를 검사했다. 캡처 계측

- 고프레임 재촬영: 262개 화면을 기록했다(캡처 간격 중앙값 34 ms). 타임스탬프 기준으로 25 fps GIF와 MP4를 출력하고 두 요청의 응답을 다시 검증했다. 고프레임 캡처 계측

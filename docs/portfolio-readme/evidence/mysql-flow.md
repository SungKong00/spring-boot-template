# MySQL 통합 테스트 근거

초기 I3 통합 검증의 관련 결과. 템플릿의 기본 Swagger·로컬 실행은 이 검사와 별도다.

| 항목 | 원문 결과 |
| --- | --- |
| Separate suite | `src/integrationTest` and Gradle `integrationTest` are outside `test`/`check`; normal build passed with `DOCKER_HOST` set to a nonexistent socket |
| Transaction | Public integration-only application service committed one row in each of two repositories; synthetic failure after the second flushed write rolled both counts back to zero |
| Tests | 19 standard JUnit + 1 OpenAPI + 3 MySQL integration tests passed; 10 initializer tests passed with integration package relocation included |

실행 명령은 `scripts/run-integration-tests.sh`다.

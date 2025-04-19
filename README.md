# platina-lab-patch-validator

이 도구는 PLATiNA :: LAB (플라티나 랩) 게임의 하단에 기재된 커뮤니티 레벨의 계산식을 기반으로 데이터 라벨링을 검증하는 프로그램입니다.

## 기능

- 주어진 데이터에서 예상 PATCH 값과 실제 PATCH 값의 차이를 계산
- 허용 오차 범위를 넘는 불일치 데이터 식별
- 커맨드 라인 인자를 통한 파라미터 조정 가능

## 사용법

```bash
python patch_validator.py [--threshold THRESHOLD] [--divisor DIVISOR] [--tolerance TOLERANCE]
```

### 인자 설명

- `--threshold`: 세부비율 조정을 위한 기준값 (기본값: 0.8499)
- `--divisor`: 세부비율 조정을 위한 나누는 값 (기본값: 13.0039)
- `--tolerance`: 허용 오차 범위 (기본값: 0.01)

### 예시

```bash
# 기본값으로 실행
python patch_validator.py

# 사용자 정의 파라미터로 실행
python patch_validator.py --threshold 0.85 --divisor 13.01 --tolerance 0.02
```

## PATCH 계산식 설명

PATCH 값은 다음 공식으로 계산됩니다:

```
PATCH = (레벨 * 4.2 * 플러스승수) * ratio_adjustment * 100
```

여기서:

- `레벨`: 플레이어의 레벨 값
- `플러스승수`: 플러스 여부에 따라 1.02(플러스인 경우) 또는 1.0(아닌 경우)
- `ratio_adjustment`: 세부비율 퍼센트값을 기반으로 한 조정값, 계산식은 아래와 같음

```
ratio_adjustment = (세부비율 - threshold) / divisor
```

이때 `ratio_adjustment`는 0과 1 사이의 값으로 제한됩니다:

- 0보다 작으면 0으로 처리
- 1보다 크면 1로 처리

### 파라미터 설명

- `threshold`: 세부비율의 기준점 (기본값: 0.8499, 약 84.99%)
- `divisor`: 세부비율 조정을 위한 나누는 값 (기본값: 13.0039)

이 공식은 세부비율이 약 85% 이상일 때부터 PATCH가 발생하기 시작하며, 세부비율이 약 98%(= 0.8499 + 13.0053)에 도달하면 최대 PATCH 값을 갖게 됩니다.

## 출처

이 계산식은 다음 PLATiNA :: LAB 커뮤니티 정보(분석) 글에서 참조되었습니다:

- [디시인사이드 플라티나 랩 마이너 갤러리 - (념요청) 패치 계산식을 알아보자](https://gall.dcinside.com/mgallery/board/view/?id=platinalab&no=3833)
- [디시인사이드 플라티나 랩 마이너 갤러리 - 패치 계산식을 알아보자 (2)](https://gall.dcinside.com/mgallery/board/view/?id=platinalab&no=3887)

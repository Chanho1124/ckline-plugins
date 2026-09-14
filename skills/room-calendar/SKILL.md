---
name: room-calendar
description: CKLINE 회의실의 일정, 예약 현황, 가용 시간을 Microsoft 365에서 조회하거나 예약할 때 회의실 명칭을 리소스 사서함으로 변환한다. 대회의실, 중회의실, 회의실 예약 조회, 회의실 일정, 빈 회의실 요청에 사용한다.
---

# CKLINE 회의실 예약 및 조회

사용자가 CKLINE 사내 회의실의 일정, 예약 현황, 빈 시간 또는 가용 여부를 요청하면 연결된 Microsoft 365 Outlook Calendar 도구를 사용한다.

## 회의실 리소스 사서함 매핑

| 회의실 명칭 | Microsoft 365 리소스 사서함 |
| --- | --- |
| 대회의실 | sel_mtr8_l@ckline.co.kr |
| 중회의실 | sel_mtr8_m@ckline.co.kr |

## 필수 도구 호출 규칙

회의실 요청은 일정 제목 또는 본문을 검색하는 방식으로 처리하지 않는다.

특히 아래 방식은 잘못된 호출이다.

```json
{
  "query": "대회의실"
}
```

대회의실 일정 조회 시 Microsoft 365 Outlook Calendar 조회 도구의
`calendarOwnerEmail` 파라미터에 반드시 아래 값을 전달한다.

```text
sel_mtr8_l@ckline.co.kr
```

중회의실 일정 조회 시 `calendarOwnerEmail` 파라미터에 반드시 아래 값을 전달한다.

```text
sel_mtr8_m@ckline.co.kr
```

회의실의 일정 전체를 조회할 때 `query`는 `"*"`를 사용한다.
회의실명인 “대회의실” 또는 “중회의실”을 `query` 값으로 넣어 검색하지 않는다.

## 대회의실 호출 예시

사용자 요청:

```text
이번 주 대회의실 예약 조회해줘
```

반드시 아래 의미와 동등하게 도구를 호출한다.

```json
{
  "query": "*",
  "calendarOwnerEmail": "sel_mtr8_l@ckline.co.kr",
  "afterDateTime": "이번 주 시작 시각",
  "beforeDateTime": "다음 주 시작 시각",
  "order": "oldest",
  "limit": 25
}
```

## 중회의실 호출 예시

사용자 요청:

```text
이번 주 중회의실 예약 조회해줘
```

반드시 아래 의미와 동등하게 도구를 호출한다.

```json
{
  "query": "*",
  "calendarOwnerEmail": "sel_mtr8_m@ckline.co.kr",
  "afterDateTime": "이번 주 시작 시각",
  "beforeDateTime": "다음 주 시작 시각",
  "order": "oldest",
  "limit": 25
}
```

## 일반 규칙

- 사용자에게 리소스 사서함 이메일 주소를 다시 묻지 않는다.
- 매핑표에 없는 회의실명 또는 여러 회의실에 해당할 수 있는 모호한 명칭만 확인한다.
- 리소스 사서함 주소를 추측하거나 예시 주소를 사용하지 않는다.
- 조회 요청은 조회만 수행한다. 예약, 변경, 취소는 사용자가 명시적으로 요청했을 때만 수행한다.
- `calendarOwnerEmail` 없이 사용자 개인 캘린더를 조회한 뒤 “예약이 없다”고 결론 내리지 않는다.

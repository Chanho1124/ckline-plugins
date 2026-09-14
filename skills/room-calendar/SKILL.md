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

## 필수 Microsoft 365 도구 호출 규칙

회의실 일정 조회에는 반드시 `outlook_calendar_search` 도구를 사용한다.

대회의실 요청일 경우, 도구 호출 JSON에 아래 필드를 반드시 포함한다.

```json
{
  "calendarOwnerEmail": "sel_mtr8_l@ckline.co.kr"
}
```

중회의실 요청일 경우, 도구 호출 JSON에 아래 필드를 반드시 포함한다.

```json
{
  "calendarOwnerEmail": "sel_mtr8_m@ckline.co.kr"
}
```

`calendarOwnerEmail`을 생략하면 사용자 개인 기본 캘린더가 조회된다.
따라서 대회의실 또는 중회의실을 언급한 요청에서 `calendarOwnerEmail` 없이
`outlook_calendar_search`를 호출하는 것은 금지한다.

회의실명은 일정 제목 검색어가 아니다. 다음 호출은 잘못된 방식이다.

```json
{
  "query": "대회의실"
}
```

회의실의 캘린더 전체를 조회할 때는 다음처럼 호출한다.

```json
{
  "query": "*",
  "calendarOwnerEmail": "sel_mtr8_l@ckline.co.kr",
  "afterDateTime": "조회 기간 시작",
  "beforeDateTime": "조회 기간 종료",
  "order": "oldest",
  "limit": 25
}
```

도구 호출 직전에 다음을 점검한다.

- 대회의실이면 `calendarOwnerEmail` 값이 `sel_mtr8_l@ckline.co.kr`인가?
- 중회의실이면 `calendarOwnerEmail` 값이 `sel_mtr8_m@ckline.co.kr`인가?
- 대상 계정을 지정하지 않은 개인 기본 캘린더 조회가 아닌가?

위 조건을 만족하지 않으면 도구를 호출하지 말고 올바른 `calendarOwnerEmail`을 포함해 다시 호출한다.

## 일반 규칙

- 사용자에게 리소스 사서함 이메일 주소를 다시 묻지 않는다.
- 매핑표에 없는 회의실명 또는 여러 회의실에 해당할 수 있는 모호한 명칭만 확인한다.
- 리소스 사서함 주소를 추측하거나 예시 주소를 사용하지 않는다.
- 조회 요청은 조회만 수행한다. 예약, 변경, 취소는 사용자가 명시적으로 요청했을 때만 수행한다.
- `calendarOwnerEmail` 없이 사용자 개인 캘린더를 조회한 뒤 “예약이 없다”고 결론 내리지 않는다.

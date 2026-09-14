---
name: room-calendar
description: CKLINE 회의실, 법인 차량, 골프·콘도 회원권 등 공용 리소스의 일정·예약·가용 시간을 Microsoft 365에서 조회하거나 예약할 때 자원 명칭을 리소스 사서함으로 변환한다. 대회의실, 중회의실, 소회의실, 부산 회의실, 브릿지, 에볼루션, 법인차량, 골프 회원권, 콘도 회원권, 예약 조회, 빈 시간 요청에 사용한다.
---

# CKLINE 공용 리소스 예약 및 조회

사용자가 CKLINE의 회의실, 법인 차량, 골프 회원권 또는 콘도 회원권의 일정, 예약 현황, 빈 시간 또는 가용 여부를 요청하면 연결된 Microsoft 365 Outlook Calendar 도구를 사용한다.

## 공용 리소스 사서함 매핑

| 사용자 표현 | Microsoft 365 리소스 사서함 |
| --- | --- |
| 대회의실 | sel_mtr8_l@ckline.co.kr |
| 중회의실 | sel_mtr8_m@ckline.co.kr |
| 부산 17층 회의실, 부산 회의실 | pus_mtr@ckline.co.kr |
| 법인차량, 카니발 6336, 카니발6336 | sel_car1@ckline.co.kr |
| 브릿지 | busan_bridge@ckline.co.kr |
| 소회의실 13A | seomeetd@ckline.co.kr |
| 소회의실 13B | seomeete@ckline.co.kr |
| 소회의실 14A | seomeetf@ckline.co.kr |
| 소회의실 4A | seomeet6@ckline.co.kr |
| 에볼루션 | busan_evolution@ckline.co.kr |
| 골프 회원권, 회원권 골프 | membershipgolf@ckline.co.kr |
| 콘도 회원권, 회원권 콘도 | membershipcondo@ckline.co.kr |

## 필수 Microsoft 365 도구 호출 규칙

공용 리소스 일정 조회에는 `outlook_calendar_search` 도구를 사용한다. 사용자가 위 매핑표의 리소스를 언급하면, 일정 제목 또는 본문에서 자원 명칭을 검색하는 방식으로 처리하지 않는다.

대신 매핑된 리소스 사서함 주소를 `calendarOwnerEmail`에 반드시 전달하고, 해당 리소스의 전체 일정을 기간으로 조회할 때 `query`는 `"*"`를 사용한다. 예를 들어 대회의실 요청에는 `calendarOwnerEmail: "sel_mtr8_l@ckline.co.kr"`를, 골프 회원권 요청에는 `calendarOwnerEmail: "membershipgolf@ckline.co.kr"`를 사용한다.

`calendarOwnerEmail` 없이 도구를 호출하면 사용자 개인 기본 캘린더가 조회될 수 있다. 위 매핑표의 리소스를 언급한 요청에서는 대상 리소스 사서함 없이 조회하고 결과가 없다고 결론내리지 않는다.

## 실행 규칙

공용 리소스 일정 조회에는 `outlook_calendar_search` 도구를 사용한다.

대회의실 요청이면 다음을 반드시 도구 호출에 넣는다.

calendarOwnerEmail: sel_mtr8_l@ckline.co.kr

회의실명으로 `query` 검색을 하지 않는다.
리소스 일정 전체 조회의 `query`는 `"*"`를 사용한다.

calendarOwnerEmail 없이 개인 기본 캘린더를 조회한 뒤 “예약이 없다”고 결론 내리지 않는다.

## 호출 예시

사용자: "이번 주 골프 회원권 예약 조회해줘"

도구 호출 의미: `query`는 `"*"`로 두고, `calendarOwnerEmail`에는 `membershipgolf@ckline.co.kr`를 전달해 해당 리소스 캘린더를 조회한다.

사용자: "오늘 대회의실 예약 조회해줘"

도구 호출 의미: `query`는 `"*"`로 두고, `calendarOwnerEmail`에는 `sel_mtr8_l@ckline.co.kr`를 전달해 해당 리소스 캘린더를 조회한다.

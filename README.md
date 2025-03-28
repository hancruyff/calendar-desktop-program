# 📆 MFC Calendar Desktop Program

**Microsoft Foundation Classes (MFC)**를 사용하여 제작된  
Windows 데스크탑용 **일정 관리 캘린더 애플리케이션**

---

## 🗂 목차

- [1. 개요](#1-개요)
- [2. 실행 화면](#2-실행-화면)
- [3. 기능 설명](#3-기능-설명)
- [4. 주요 코드 구성](#4-주요-코드-구성)
- [5. 프로젝트 구조](#5-프로젝트-구조)
- [6. 기술 스택](#6-기술-스택)
- [7. 학습 포인트](#7-학습-포인트)

---

## 1. 📌 개요

- C++ 기반 **MFC 응용프로그램 구조**를 학습하며 개발한 일정 캘린더 프로그램입니다.
- 날짜 선택, 일정 등록/삭제, 파일 입출력을 통한 일정 유지가 가능하도록 구현되었습니다.
- `CDialog 기반`의 모달 대화상자 인터페이스로 사용자와 상호작용합니다.

---

## 2. 🖼 실행 화면
<img src="https://github.com/user-attachments/assets/219dbd23-b77b-4175-99e1-70c52891f6ac" width="599" height="456"/>
<img src="https://github.com/user-attachments/assets/b6f10a39-d3b9-4498-a94f-fd8e502f39d5" width="599" height="450"/>
<img src="https://github.com/user-attachments/assets/8383919a-a830-4044-bfb4-b1bcd72c0de7" width="597" height="451"/>
<img src="https://github.com/user-attachments/assets/e01d8b65-4ac9-4ca8-a476-086f2ce42caa" width="602" height="453"/>
<img src="https://github.com/user-attachments/assets/71eaacb4-f6d1-468f-b35b-acc9d7fef5a2" width="300" height="200"/>
<img src="https://github.com/user-attachments/assets/9e089e4a-5a31-4723-9053-722596de7e5e" width="300" height="200"/>
<img src="https://github.com/user-attachments/assets/df296a04-eeed-40b9-b9f8-1d3deaaf33ce" width="300" height="200"/>
<img src="https://github.com/user-attachments/assets/8b2bad34-1477-472f-a5af-be43bd1d6f2d" width="300" height="200"/>

---

## 3. ⚙️ 기능 설명

| 기능         | 설명 |
|--------------|------|
| 📅 날짜 선택  | 사용자 입력을 통해 특정 날짜 클릭 및 선택 |
| ✏️ 일정 입력  | 선택한 날짜에 일정 텍스트 작성 및 등록 가능 |
| 🗑 일정 삭제  | 기존 일정 선택 후 삭제 버튼으로 제거 |
| 💾 파일 저장  | `TXT 파일`로 일정 내용 저장 / 프로그램 재실행 시 로드 가능 |
| 🔍 일정 조회  | 날짜를 클릭하면 해당 날짜의 일정 출력 |

---

## 4. 🔍 주요 코드 구성

### 1. 날짜 선택 핸들링

```cpp
void CMFCApplication3Dlg::OnBnClickedSelectDate()
{
    CTime currentDate;
    m_DatePicker.GetTime(currentDate);
    CString dateStr = currentDate.Format(_T("%Y-%m-%d"));
    m_SelectedDate = dateStr;
    UpdateScheduleView(); // 선택된 날짜의 일정 표시
}
```

### 2. 일정 등록

```cpp
void CMFCApplication3Dlg::OnBnClickedAddSchedule()
{
    CString input;
    m_ScheduleInput.GetWindowText(input);
    scheduleMap[m_SelectedDate].Add(input); // 날짜별 일정 리스트에 추가
    SaveScheduleToFile();
    UpdateScheduleView();
}
```

### 3. 일정 저장 및 불러오기(파일입출력)

```cpp
void CMFCApplication3Dlg::SaveScheduleToFile()
{
    CStdioFile file(_T("schedule.txt"), CFile::modeCreate | CFile::modeWrite);
    for (auto& date : scheduleMap)
    {
        for (auto& line : date.Value)
        {
            CString entry = date.Key + _T("|") + line;
            file.WriteString(entry + _T("\n"));
        }
    }
    file.Close();
}

void CMFCApplication3Dlg::LoadScheduleFromFile()
{
    CStdioFile file(_T("schedule.txt"), CFile::modeRead);
    CString line;
    while (file.ReadString(line))
    {
        int sep = line.Find(_T("|"));
        CString date = line.Left(sep);
        CString schedule = line.Mid(sep + 1);
        scheduleMap[date].Add(schedule);
    }
    file.Close();
}
```

---

### 5. 📁 프로젝트 구조
```
MFCApplication3/
├── MFCApplication3.cpp / .h           → 애플리케이션 클래스
├── MFCApplication3Dlg.cpp / .h        → 주요 다이얼로그 클래스 (기능 구현)
├── resource.h                         → 리소스 매핑 헤더
├── stdafx.h / stdafx.cpp              → 선행 컴파일 헤더
├── test/                              → 실행 데이터 폴더
└── schedule.txt                       → 일정 저장 파일
```
---

### 6. 🛠 기술 스택
<img src="https://img.shields.io/badge/C%2B%2B-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white"/> <img src="https://img.shields.io/badge/MFC-0078D7?style=for-the-badge&logo=windows&logoColor=white"/> <img src="https://img.shields.io/badge/FileIO-%23003B57?style=for-the-badge"/>

C++ / MFC: Microsoft Foundation Class를 기반으로 UI 및 이벤트 처리

CDialog: 모달 대화상자 기반 인터페이스

CString / CTime / CStdioFile: MFC 전용 문자열, 날짜 처리 및 파일 입출력 사용

### 7. 📚 학습 포인트

✔️ MFC 기반의 Windows GUI 구조 이해 (메시지 루프, 다이얼로그)

✔️ 날짜 기반 데이터 구조 및 Map<날짜, 일정리스트> 설계

✔️ 파일 기반 간단한 로컬 DB 구성 및 일정 유지

✔️ MFC 컨트롤 (CEdit, CListBox, CDateTimeCtrl) 활용

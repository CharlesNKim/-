# MsgBox2
메세지 박스를 사용한 예제
* 메세지 박스는 MessageBox() 와 AfxMessageBox() 두가지가 있다. 전자는 Cwnd()의 멤버 함수이고 후자는 MFC Library에 의해 제공되는 전역 함수이다.
### 구현
리소스 뷰에서 edit Control을 배치하고 컨트롤 변수로 등록 한다. 버튼 리스너에 아래와 같은 코드를 삽입한다.
~~~C++
        int iResults;
	iResults = AfxMessageBox(_T("Yes / No 버튼을 누르셨습니다."), (MB_YESNO | MB_ICONEXCLAMATION));

	if(iResults == IDYES) {
		m_strResult = _T("YES  버튼을 눌렀습니다.");
		UpdateData(FALSE);
	}
	if (iResults == IDNO) {
		m_strResult = _T("NO 버튼을 눌렀습니다.");
		UpdateData(FALSE);
	}
~~~
######설명
> iResults 변수는 AfxMessageBox()가 반환하는 상수를 가지게된다. 이 상수는 메세지 박스에서 클릭한 버튼과 관련있다.
> m_strResult 변수는 edit Control의 컨트롤 변수이다.
> **UpdateData()** 함수는 **인자가 FALSE일떄 컨트롤 변수의 값을 윈도우에 반영**한다. 반대로 TRUE 일때 윈도우에 있는 값을 컨트롤 변수에 넣는다.
---
# MClock
CTIME 클래스와 윈도우 함수를 이용하여 현재 시간과 날자를 표현한 예제
* **CRect**
    윈도우 구조체 Rect과 유사한 객체인 **CRect**은 **화면상의 사각형 영역의 좌표를 저장**하기위한 4개의 **변수**(top, bottom, left, right)를 가진다. 또 해당 사각형의 높이와 폭을 구할 수 있는 함수 Height()과 Width()가 기본으로 존재한다.
* **GetSystemMetrics()**
    GetSystemMetrics()는 시스템 환경 설정값 함수다. 파라미터로 **SM_CYSCREAN**, **SM_CXSCREAN** 을 각각 넣으면 현재 **모니터의 (해상도)크기**를 알 수 있다.
* **GetWindowRect()**
    GetWindowRect()는 현재 위도우 프로그램의 화면상 위치를 알 수 있는 함수이다. **파라미터로 CRect 객체**를 넘겨주면 거기에 윈도우 프로그램의 좌표를 돌려준다.
* **MoveWindow**
    MoveWindow는 주어진 **좌표로 어플리케이션을 이동**시킨다. 두개 파라미터중 첫번째 값은 **화면상의 좌표**이며, 두번째 값은 WM_PAINT 메시지 발생시 화면에 그림을 다시 그릴지 아닐지를 결정한다.

### 구현
리소스 뷰에 화면을 구성하고 헤더 파일에 변수를 선언 하였다.
~~~C++
    CRect screen; // 화면 크기 저장 변수
    int vsize, hsize; // 프로그램의 폭과 높이 저장 변수

    UINT htimer; // 타이머를 위한 변수
~~~
이후 OnInitDialog() 함수에서 변수를 초기화 시킨다.
~~~C++
    CRect rect;
    // 헤더에서 만든 screen 변수
    screen.top = 0;
    screen.left = 0;
    screen.right = ::GetSystemMetrics(SM_CXSCREEN); // 스크린 크기
    screen.bottom = ::GetSystemMetrics(SM_CYSCREEN); // 스크린 크기

    // 1 : 타이머 식별자, 1000 : 타이머를 일으키는 시간, NULL : 콜백함수
    htimer = SetTimer(1, 1000, NULL);
    
    GetWindowRect(rect); // 프로그램의 화면상 좌표값을 준다.
    vsize = rect.Width(); // 프로그램의 가로 크기
    hsize = rect.Height(); // 프로그램의 세로크기
~~~

CMClockdlg 클래스에 WM_TIMER 메시지 처리 함수를 추가한다.
~~~C++
void CMClockDlg::OnTimer(UINT_PTR nIDEvent)
{
    CTime gct = CTime::GetCurrentTime();
    CString strDate;
    CString strTime;
    
    strDate.Format(_T("%d 년 %d 월 %d 일"), gct.GetYear(), gct.GetMonth(), gct.GetDay());
    GetDlgItem(IDC_STATIC_DATE)->SetWindowText(strDate);

    strTime.Format(_T("%d 시 %d 분 %d 초"), gct.GetHour(), gct.GetMinute(), gct.GetSecond());
    GetDlgItem(IDC_STATIC_TIME)->SetWindowText(strTime);

    Invalidate(); // 새로 고침
    CDialogEx::OnTimer(nIDEvent);
}
~~~

마우스를 피해 윈도우가 움직이기 위해 WM_SETCUSOR 메시지 처리 함수를 추가한다.
> 윈도우에 마우스가 올라왔을때 동작하는 함수
~~~C++
BOOL CMClockDlg::OnSetCursor(CWnd* pWnd, UINT nHitTest, UINT message)
{
    RECT winpos;
    int x = rand() % (screen.right - vsize);
    int y = rand() % (screen.bottom - hsize);
    winpos.top = y;
    winpos.bottom = y + hsize;
    winpos.left = x;
    winpos.right = x + vsize;
    MoveWindow(&winpos, TRUE);
    return CDialogEx::OnSetCursor(pWnd, nHitTest, message);
}
~~~

마지막으로 프로그램 종료시 TIMER를 삭제하기 위해 WM_CLOSE 메시지 처리 함수를 추가한다.
~~~C++
void CMClockDlg::OnClose()
{
    // 타이머 삭제 함수
    KillTimer(htimer);
    CDialogEx::OnClose();
}
~~~
---

# 클래스 추가 예제
리소스 뷰에서 **새로운 다이얼로그를 먼저 추가**한다. 이후 추가한 다이얼로그에서 마우스 오른쪽 클릭-클래스 추가를 선택하고 새 다이얼로그의 클래스 이름을 입력한다.
솔루션 탐색기에서 .cpp 파일과 .h 파일이 새로 생긴것을 확인한다.
원래 다이얼로그 헤더파일을 열어서 아래와 같은 코드를 추가한다.
######구현
~~~C++
    // 추가한 다이얼로그 클래스의 헤더
    #include "CInputDlg.h"

    // CClassAdd1Dlg 대화 상자
    class CClassAdd1Dlg : public CDialogEx
    {
    // 생성입니다.
    public:
    CClassAdd1Dlg(CWnd* pParent = nullptr);	// 표준 생성자입니다.
    // 정의한 다이얼로그 클래스 변수
    CInputDlg m_InputDlg;

~~~
> CInputDlg가 새로만든 다이얼로그 클래스 이름이다. 헤더를 추가해주고 변수를 정의했다. 이 변수를 가지고 새로운 다이얼로그를 띄운다.

이후 버튼 메시지 처리함수에 대화 상자를 띄우는 코드를 추가한다.
~~~C++

void CClassAdd1Dlg::OnBnClickedButtonInput()
{
    // 아래 DoModal() 함수가 새 다이얼로그를 띄우는 함수다.
    m_InputDlg.DoModal();
}
~~~
---




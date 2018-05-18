# MFC에 대하여
### 윈도우
1. 윈도우의 정의
  프로그램이 출력 결과를 내보내고 사용자로부터 입력을 받아 들이는 화면상의 영역

2. 윈도우 클래스
  모든 윈도우들은 윈도우 클래스로부터 만들어진다. 윈도우 클래스는 윈도우를 만들기 위한 틀.
  생성될 윈도우의 여러가지 특징을 모아놓은 구조체.

3. 윈도우 클래스 종류
  * 시스템 전역 클래스 : os가 부팅 될때 등록. 컨트롤(Button, edit, scrollbar)을 만들때 사용
  * 응용프로그램 전역 클래스 : dll에 의하여 등록. 프로세스의 모든 모듈에서 사용가능.
  * 응용프로그램 로컬 클래스 : 응용프로그램 자신이 메인 윈도우나 차일드 또는 커스텀 컨트롤을 만들기 위해 프로그램 선두에서 등록한 클래스.
---
### MFC의 계층
  모든 클래스는 CObject 클래스를 상속 받는다.
  Java 에서 Object클래스와 비슷하다.
  CObject 클래스 하위에는 CCmdTarget클래스가 있다.
  CCmdTarget 클래스 하위에는 CWinApp, CWnd, CDocument, CDocTemplelate 의 네가지 클래스가 있다.
  CWnd 클래스 하위에는 CFrameWnd, CDialog, CView, CCtrlView 클래스가 있다.
  CDocument 클래스 하위에는 CSingleDocTemplate, CMultiDocTemplate 클래스가 있다.

* CObject
  - CCmdTarget
    + CWinApp
    + CWnd
      = CFrameWnd
      = CDialog
      = CView
      = CCtrlView
    + CDocument
    + CDocTemplelate
      = CSingleDocTemplate
      = CMultiDocTemplate

### MFC 기반 클래스 설명

1. CObject
  MFC 최상위 클래스. 메모리 클래스를 설정하는 기능을 가짐.
  프로그램 실행중 객체 정보를 알아내고, 객체 상태를 점검

2. CCmdTarget
  명령 관련 클래스. 윈도우 메시지 응잠을 위한 기본 클래스.

3. CWinApp
  프로그램 시작, 종료를 제어. 윈도우 앱을 생성하는 기본 클래스.

4. CWnd
  Window Object를 클래스로 구현한것. 윈도우의 크기, 모양, 위치 등의 상태를 제어하거나 윈도우 메세지를 처리하는 클래스.
  추가 : CWnd는 MFC 클래스인데, 윈도우의 거의 모든 API 함수들을 몽땅 집어넣은 클래스 이다. 이 클래스 안에는 멤버 변수로 윈도우 핸들을 가지고 있어서 CWnd로 파생받은 모든 클래스들은 윈도우로 볼 수 있다.  버튼이나 에디트, 대화상자 들이 모두 CWnd를 파생받은 걸 볼 수 있다.
  1.함수에서 CWnd 가 필요한데 HWND밖에 없을 때
  CWnd* pWnd - CWnd::FromHandle(HWND);
  2.HWND가 필요한데 CWnd 밖에 없을때
  HWND hWnd - pWnd- m_hWnd;

5. CDocument
  파일로부터 데이터를 읽고 저장하는 기능과 새로운 데이터를 생성, 수정 병경 처리하는 클래스.
  데이터 입출력 관련 클래스.

6. CDocTemplelate
  Document Template에 대한 기본 클래스.
  단일 문서와 관련된 CSingleDocTemplate.
  다중 문서와 관련된 CMultiDocTemplate.

7. CFrameWnd
  윈도우 클래스. 윈도우 외곽 경계를 정의하는 클래스.
  CWnd를 상속받아 윈도우 메시지 처리

8. CDialog
  다이얼로그를 생성하거나 다이얼로그를 가진 컨트롤과 멤버변수 사이의 데이터 교환 클래스

9. CView
  작업영역을 나타내는 클래스로서 데이터를 화면에 보일 수 있도록 처리.
---
### 핸들에 대한 이해
  헨들이란 구체적인 어떤 대상에 붙여진 번호. 문법적으로는 32비트의 정수값.
  도스에서는 거의 유일하게 파일 핸들이 사용되었고 윈도우즈에서는 여러가지 종류의 핸들이 사용된다.
  만들어진 윈도우에도 hWnd 핸들을 붙여 번호로 관리한다.
  대상의 구분을 위해 문자열보다는 정수값이 빠르다.

#### 핸들의 특징
1. 핸들은 정수값이며 대부분 32비트 값이다. 핸들의 목적은 오로지 구분을 위한것이고 중복되지 않아야 한다.
2. 핸들은 os가 발급하고 사용자는 쓰기만 한다.
3. 같은 종류의 핸들은 절대 중복된 값을 가지지 않는다.
4. 다른 종류의 핸들은 중복된 값을 가질수도 있다.
5. 핸들은 정수형이므로 실제 값을 가지지만 그것의 값을 알필요는 없다. 핸들은 변수를 만들어 쓰고 나서 버린다.
6. 윈도우즈에서 핸들은 예외없이 접두어 'h'로 시작되며 핸들값을 저장하기위한 별도의 데이터형까지 정의되어 있다. ex) HWND, HPEN, HBRUSH, HDC 등 모두 usnigned int 값이다.
---
### DC와 GDI Object
1. DC
화면에 무엇인가를 출력하기 위해서는 반드시 윈도우즈 os로부터 화면을 사용할 수 있는 권한, Device Context를 얻어야한다. DC는 화면이나 프린터, 플로터 등 출력 장치에 문자나 그림을 표시하기 위한 정보를 지닌 구조체이다.
  * CDC Class :  DC에 대한 기초 클래스 출력에 관계된 대부분의 맴버함수 포함. 4개의 파생 클랫 존재.
  * CWindowsDC : CWindowsDC는 캡션바, 메뉴바, 상태바 등 Non Client영역을 포함한 전체 윈도우 DC관리.
  * CClientDC : CClientDC는 캡션바, 메뉴바, 상태바 등을 제외한 클라이언트 영역만을 표시하는 DC관리.
  * CPaintDC : WM_PAINT 메시지가 발생했을때 다시 그려저야할 영역에 대한 DC를 관리. WM_PAINT 메시지 핸들러인 OnPaint()함수에서 사용.
    + Invalid Region : 여러개의 프로그램 사용시 겹쳐서 가려졌던 부분. 복원할때 전체 그림을 다시 그릴필요없이 가려졌던 Invalid Region만 다시 그려주면 효율적.
2. GDI Object
DC가 도화지라면 그림을 그리는 도구가 GDI Object 라고 할 수 있다. GDI Object 사용시 CDC클래스의 SelectObject() 맴버함수가 아래와 같이 사용된다.
~~~C++
CClientDC cd(this);
CPen myPen, *pOldPen;
myPen.Create(PS_SOLID, 1, RGB(0, 0, 255));
pOldPen = dc.SelectObject(&myPen);
...
dc.SelectObject(pOldPen);
myPen.DeleteObject();
~~~
1, 2, 3 행과 같이 Pen Object를 설정하고 4행에서 설정한 Pen Object를 사용하는 SelectObject()함수를 이용한다. 5행에서 출력과정이 이루어지고 사용이 끝났을경우 예정 Object로 변경해주고 메모리는 삭제한다.

---
### WinMain() 함수의 이해
WinMain 함수의 원형은 아래와 같다
~~~C++
int APIENTRY WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpszCmdParam, int nCmdShow)
~~~
1. APIENTRY :  __stdcall 형 호출 규격을 사용한다는 의미. 이건 나중을 기약하자.
2. hInstance : 프로그램의 인스턴스 핸들.
3. hPrevInstance : 바로앞에 실행된 현재 프로그램의 인스턴스 핸들.
4. ipCmdLine : 명령행으로 입력된 파라미터. 도스의 argv인수와 같음.
5. nCmdShow : 프로그램이 실행될 형태이며 최소화, 보통모양등이 전달됨.
ex) 메모장을 두개 켰을때 모두 메모장 프로그램이지만 os는 각각 다른 메모리를 사용하는 다른 프로그램으로 인식한다. 각 메모장은 서로 다른 인스턴스 핸들을 가지고 있으면 이것으로 구분된다.

#### 윈도우 클래스
WinMain 함수에서 하는 가장 중요한 일은 윈도우를 만드는 일이다.
윈도우를 만드려면 윈도우 클래스르 먼저 등록한 후 CreateWindow 함수를 호출해야 한다.
모든 윈도우는 윈도우 클래스를 기반으로 만드렁지며, 윈도우 클래스에서는 만들어질 윈도우의 여러가지 특성을 정의한다. 윈도우 클래스는 window.h 에 아래와 같이 정의되어 있는 구조체이다.
~~~C++
typedef struct tagWNDCLASS
{
    UINT        style;
    WNDPROC     lpfnWndProc;
    int         cbClsExtra;
    int         cbWndExtra;
    HINSTANCE   hInstance;
    HICON       hIcon;
    HCURSOR     hCursor;
    HBRUSH      hbrBackground;
    LPCSTR      lpszMenuName;
    LPCSTR      lpszClassName;
} WNDCLASS;
~~~
총 열개의 멤버를 가지고 있다.

##### Style
윈도우가 어떤 형태를 가질것인가를 지정하는 멤버.
보톤 CS_HREDRAW 와 CS_VREDRAW를 '|' 연산자로 연결하여 사용.
윈도우의 수평 또는 수직 크기가 변할경우 윈도우를 다시 그린다는 의미이다.

##### ipfnWndProc  중요!
윈도우의 메시지 처리 함수를 지정한다. 메세지가 발생할때마다 여기서 지정한 함수가 호출되며 이 함수가 모든 메시지를 처리한다.
WinMain에서는 윈도우를 만들고 화면에 출력하기만 할 뿐이며 대부분의 일은 WndProc에서 이루어진다. WinMain 함수의 바로 윗부분에 WndProc 함수의 원형이 선언되어 있다.
~~~C++
LRESULT CALLBACK WndProc(HWND, UINT, WPARAM,LPARAM);
~~~

##### cbClsExtra, cbWndExtra
예약 영역. 윈도우즈가 내부적으로 사용하는 특수한 목적의 여분의 공간. 사용하지 않을 경우에는 0으로 지정.

##### hInstance
이 윈도우 클래스를 사용하는 프로그램의 번호이며 이 값은 WinMain의 인수로 전달된 hInstance값을 그대로 대입해주면 된다.

##### hIcon, hCursor
이 윈도우가 사용할 마우스 커서와 최소화 되었을경우 출력할 아이콘 지정.
LoadCursor() 함수와, LoadIcon()함수를 사용하여 지정.

##### hbrBackground
윈도우의 배경 색상지정. 정확하게는 **윈도우의 배경 색상을 채색할 브러시를 지정하는 맴버.**
GetStockObject() 함수를 사용하여 윈도우에서 기본적으로 제공하는 브러시를 지정한다.
가장 일반적인 흰색배경(WHITE_BRUSH)

##### lpszMenuName
이 프로그램이 사용할 메뉴 지정. 메뉴는 프로그램 코드로 구현하지 않고 리소스 에디터에 의해 별도로 만들어진 후 링크시 합쳐진다. 사용하지 않을경우 NULL.

##### lpszClassName  중요!
윈도우의 클래스 이름을 정의한다. 여기서 지정한 이름이 CreateWindow 함수에 전달되어지고 CreateWindow 함수는 윈도우 클래스에서 정의한 특성값을 참조하여 윈도우를 만든다.
윈도우 클래스의 이름은 **보통 실행파일의 이름과 일치시켜 작성**한다.

#### 윈도우 클래스의 등록
위와 같이 윈도우 클래스를 정의한 후 RegisterClass() 함수를 호출하여 윈도우 클래스를 등록한다. 원형은 아래와 같다.
~~~
ATOM REgisterClass(CONST WNDCLASS *IpWndClass);
~~~
RegisterClass() 함수의 파라미터로 WndClass 구조체의 **번지**를 넘겨주면 된다.
이러한 특성을 가진 윈도우를 사용하겠다는 **등록과정**이다.
~~~C++
  WNDCLASS WndClass;

	WndClass.style = CS_HREDRAW | CS_VREDRAW;
	WndClass.lpfnWndProc = WndProc;
	WndClass.cbClsExtra = 0;
	WndClass.cbWndExtra = 0;
	WndClass.hInstance = hInstance;
	WndClass.hIcon = LoadIcon(NULL, IDI_APPLICATION);
	WndClass.hCursor = LoadCursor(NULL, IDC_ARROW);
	WndClass.hbrBackground = (HBRUSH)GetStockObject(WHITE_BRUSH);
	WndClass.lpszMenuName = NULL;
	WndClass.lpszClassName = TEXT("Class");
	if (!RegisterClass(&WndClass)) {
		return 1;
	}
~~~

#### 윈도우 생성
윈도우 생성을 위해서 CreateWindow() 함수를 사용한다. 원형은 아래와 같다.
~~~
HWND CreateWindow(lpszClassName, lpszWindowName, dwStyle, x, y, nWidth, nHeight, hwndParent, hmenu, hinst, lpvParam)
~~~
##### lpszClassName
생성하고자 하는 윈도우의 클래스를 지정하는 문자열. 앞에서 정의한 WndClass구조체의 lpszClassName을 동일하게 기입해준다.

##### lpszWindowName
윈도우의 타이틀 바에 나타날 문자열.

##### dwStyle
윈도우의 형태를 지정하는 인수. 윈도우가 경계선을 가질것인가, 타이틀바를 가질것인가, 스크롤바의 유무등등을 상세하게 지정할 수있다. or 연산을 이용해 여러 값을 한번에 보낸다.

##### x, y, nWidth, nHeight
윈도우의 크기와 위치를 지정하며 픽셀 단위를 사용한다. x, y 좌표는 메인 윈도우의 경우 전체화면을 기준으로 하며, 차일드 윈도우는 무보윈도우의 좌상단을 기준으로 한다.
정수값을 바로 지정해도 되며 CW_USERDEFAULT를 사용하면 적절히 맞춰준다.

##### hWndParent
부모 윈도우가 있을경우 부모윈도우의 핸들을 지정해 준다. 없으면 NULL

##### hmenu
윈도우에서 사용할 메뉴의 핸들을 지정한다. WndClass에도 메뉴를 지정할 멤버가 있는데 그것은 그 윈도우 클래스를 기반으로 하는 모든 윈도우에서 사용되는 메뉴이고, 여기서 지정된것은 현재 CreateWindow() 함수로 만들어지는 윈도우에서만 사용된다. NULL로 설정하면 WndClass에서 설정한 값을 그대로 사용한다.

##### hinst
윈도우를 만드는 주체인 프로그램의 핸들을 지정한다. WinMain의 인수로 전달된 hInstance를 대입해주면된다.

##### lpbParam
CREATESTRUCT 라는 구조체의 번지이며 특수한 목적에 사용한다. 보통은 NULL값을 사용한다. 이것도 나중에 알아보자!!!

> CreateWindow 함수는 윈도우에 관한 모든 정보를 메모리에 만든후 윈도우 핸들을 리턴해준다. 넘겨지는 윈도우 핸들은 HWND hWnd에 저장되고 윈도우를 참조하는 모든 함수의 인수로 사용된다.

#### 화면에 보이는 윈도우
CreateWindow() 함수로 만든 윈도우는 메모리상에만 있으며 출력되지는 않았다. 메모리에 만들어진 윈도우를 출력하기위해 **ShowWindow()** 함수를 사용한다. 원형은 아래와 같다
~~~C++
BOOL ShowWindow(hWnd, nCmdShow); 
~~~ 
파라미터 hWnd는 화면으로 출력하고자 하는 윈도우의 핸들이며, CreateWindow()함수가 리턴한 핸들을 그대로 넘겨주면 된다. nCmdShow는 윈도우를 화면에 출력하는 방법을 지정하며 아래와 같은 매크로 상수들이 정의되어있다.
|매크로 상수|의미|
|:---------|---:|
|SW_HIDE|윈도우를 숨긴다|
|SW_MINIMIZE|윈도우를 최소화 시키고 활성화시키지 않는다.|
|SW_RESTORE|윈도우를 활성화 시킨다.|
|SW_SHOW|윈도우를 활성화시켜 보여준다.|
|SW_SHOWNORMAL|윈도우를 활성화시켜 보여준다.|

nCmdShow 파라미터네 값은 WinMain함수의 인수로 전달된 nCmdShow를 그대로 넘겨주면 된다.
코드는 아래와 같다.
~~~C++
  HWND hWnd = CreateWindow(
    lpszClass,
    lpszClass,
    WS_OVERLAPPEDWINDOW,
    CW_USEDEFAULT,
    CW_USEDEFAULT,
    CW_USEDEFAULT,
    CW_USEDEFAULT,
	  NULL,
    (HMENU)NULL,
    hInstance,
    NULL);
	ShowWindow(hWnd,nCmdShow);
~~~
#### 윈도우가 만들어지는 과정 마무리
1. WndClass 정의 -> 윈도우의 기반이 되는 클래스를 정의한다.
2. CreateWindow -> 메모리상에 윈도우를 만든다.
3. ShowWindow -> 윈도우를 화면에 표시한다.
4. 메시지 루프 -> 사용자로부터의 메시지를 처리한다.

#### 메시지 루프
윈도우즈는 메시지 구동 시스템이다. 프로그램의 실행 순서가 명확힣 정해져 있지 않고 상황에 따라 순서가 달라진다. 상황은 어떤 메시지가 주어졌는가를 말한다.
> 메시지 : 사용자나 시스템 내부적인 동작에 의해 발생된 일체의 변화에 대한 정보.
> ex) 버튼 클릭, 키보드 누름 등등.
이런 메시지를 처리하는 부분을 메시지 루프라고 하며 WinMain() 함수 끝부분에 아래 코드와 같이 존재 한다.
~~~C++
while(GetMessage(&msg,0,0,0)) {
	TranslateMessage(&msg);
	DispatchMessage(&msg);
}
~~~

메시지 루프는 세개의 함수 호출로 이루어져 있으며 전체 루프는 while 문으로 싸여져 있다.
~~~
BOOL GetMessage( LPMSG lpMsg, HWND hWnd, UINT wMsgFilterMin, UINT wMsgFilterMax); 
~~~
GetMessage() 함수는 시스템이 유지하는 메시지 큐에서 메시지를 읽어들인다. 읽어들인 메시지는 첫번째 파라미터가 지정하는 MSG 구조체에 저장된다. 
또 읽어들인 메시지가 WM_QUIT(프로그램 종료 메시지)일 경우에 False를 리턴하고 그외에는 TRUE를 리턴한다. 
따라서 프로그램 종료 전까지 무한 반복문이 실행된다. 나머지 세개의 파라미터는 읽어들일 메시지의 범위를 지정하는데 사용된다.(별로 안중요)

~~~C++
BOOL TranslateMessage( CONST MSG *lpMsg); 
~~~
TranslateMessage() 함수는 키보드 눌림(WM_KEYDOWN)과 떨어짐(WM_KEYUP)이 연속적으로 발생할때 문자가 입력되었다는 메시지(WM_CHAR)를 만드는 역할을 한다.

~~~
LONG DispatchMessage( CONST MSG *lpmsg); 
~~~
메시지 큐에서 꺼낸 메시지를 프로그램의 메시지 처리함수(WinProc)으로 전달한다.

#### 메시지 구조체
~~~C++
typedef struct tagMSG
{
    HWND        hwnd;
    UINT        message;
    WPARAM      wParam;
    LPARAM      lParam;
    DWORD       time;
    POINT       pt;
} MSG;
~~~
|멤버|의미|
|:---|---:|
|hwnd|메시지를 받을 윈도우 핸들|
|message|어던 종류의 메시지인가를 나타냄. 가장 중요한 값|
|wParam|전달된 메시지에 대한 부가적인 정보|
|lParam|전달된 메시지에 대한 부가적인 정보|
|time|메시지가 발생한 시간|
|pt|메시지가 발생했을떄 마우스 위치|

결국 GetMessage()함수가 읽은 메시지를 MSG형 구조체에 대입해주며 이 구조체는 DispatchMessage()함수에 의해 메시지 처리함수 WinProc으로 전달된다.

---
### WndProc() 함수의 이해
메시지 처리 함수랑 메시지가 발생할 때 프로그램의 반응을 처리하는 일을 하며 WinMain 함수와는 별도로 WndProc이라는 이름으로 존재한다. 윈도우 프로시저(Window Procedure)라는 뜻이다.
WndProc은 WinMain에서 호출하는 것이 아닌 윈도우즈에 의해 호출되는 콜백 함수이다.
(**WinMain 내의 메시지 루프는 메시지를 처리함수로 보내주기만 할뿐이다.**)
~~~
LRESULT CALLBACK WndProc(HWND hWnd,UINT iMessage,WPARAM wParam,LPARAM lParam);
~~~
WndProc() 함수의 파라미터는 4개이며 MSG 구조체의 멤버와 동일하다.
(여기서 질문. 구조체의 맴버와 파라미터가 같으면 그냥 MSG개체를 파라미터로 받으면 되는거 아닌가?)
hwnd 는 메시지를 받을 윈도우의 핸들.
iMessage 는 어떤 종류의 메시지인가, 즉 어던 변화가 발생했는가에 대한 정보다.
ex) iMessage 가 WM_MOVE이면 윈도우의 위치가 변경되었음을 알린다.
wParam, iParam은 iMessage의 메시지에 다른 부가적인 정보를 가진다.
ex1) 마우스가 눌렸을때 화면의 어디쯤인가, 그때의 키보드 상황은 어떤가.
ex2) 키보드가 눌렸을때 어떤 키가 입력되었는가

WndProc의 구조는 일반적으로 아래와 같다.
~~~C++
switch(iMessage)
{
	case Msg1:
		처리1;
		return 0;
	case Msg2:
		처리2;
		return 0;
	case Msg3:
		처리3;
		return 0;
	default:
		return DefWindowProc(...);
}
~~~
WndProc() 함수는 메시지를 처리했을경우 반드시 0을 리턴해줘야한다.
dEfWindowProc() 함수가 메시지를 처리했을때 이 함수가 리턴한 값을 리턴해 주어야한다.

---


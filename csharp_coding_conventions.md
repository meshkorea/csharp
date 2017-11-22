Architecture
-------------

기능에 맞게 모듈과 레이어를 구성하고 관련 모듈별로 솔루션 폴더로 묶는다.
> UI와 비즈니스 로직 관련 모듈은 반드시 분리하고 UI모듈에서 DB나 네트워크같은 의존성 높은 코드를 작성하지 않는다.

![](image/architecture.jpg?raw=true)

관련 모듈은 루트명을 통일하고 . 을 사용하여 하위명을 생성한다.

하나의 타입 (클래스, 인터페이스, enum등) 은 동일한 이름의 하나의 소스파일을 사용한다.
> 내부타입에 대해서는 해당 파일에 포함될 수 있다.

Config 파일을 통해 빌드 환경을 관리한다.
> Slow Cheetah

![](image/slowcheetah.jpg?raw=true)

----------

Naming Conventions
-------------
Pascal Casing, Camel Casing 을 사용한다.
> **Pascal Case :** 클래스, 메소드, 프로퍼티, 이벤트, 상수 읽기전용 적정 변수, 인터페이스, enum

    public class MyClass
	{
	    public const int MaxCount = 30;
	    public static readonly string Tag;
	    public int Id { get; set; }
	    public event EventHandler Changed;
	       
	    public void TestMethod()
	    {
	    }
	}
	
	public interface IMyClass
	{
	}
	
	public enum OrderStatus
	{
	    Submitted,
	    Assigned,
	    Pickup,
	    Delivered
	}


> **Camel Case:** 변수, 메소드 파라메터

    public class MyClass
	{
	    public string className;
	    public int classNumber;
	       
	    public void CallTestMethod()
	    {
	        int startIndex = 1;
	        int endIndex = 2;
	 
	        TestMethod(startIndex, endIndex);
	    }
	 
	    public void TestMethod(int startIndex, int endIndex)
	    {
	    }
	}


헝가리안 표기법은 사용하지 않는다.
> String m_name, int nCount

접근제한자는 항상 명시한다.
> protected, public , private, internal

단어를 축약하지 말고 명시적으로 작성한다.
> - int cnt, string addr
> - public int GetCnt()
> - 반복문에서는 예외 ( for int i = 0; i < count; i++ )
> - 알려진 용어는 예외적으로 사용하고 2자 이상일 경우 Pascal Casing이나 Camel Casing을 사용
>   ( UserInterface -> UI )

private 멤버변수는 _ 로 시작한다.
> private string _userNumber;

멤버 호출 시 this는 명확한 호출이 필요한 경우가 아닌 이상 붙이지 않는다.
제네릭의 파라메터 타입이 복수가 아닌 경우 T와 같은 한 문자를 사용한다.
bool 값을 리턴하는 경우 is, has, can 등의 접두어를 사용한다.
메소드는 행위를 나타냄으로 동사를 사용한다.
> Addition -> Add

변수, 프로퍼티는 명사를 사용한다.
> createTable -> newTable

이벤트는 동사형으로 표현한다.
> Completion -> Completed

타입 정의 시 불필요한 접미어를 붙이지 않는다.
> - OrderStatusEnum -> OrderStatus
> - 파생클래스에서는 접미어를 붙인다.
> Exception, Collection, EventArgs, Delegate, Attribute, Command, Behavior, Converter
> NotFound -> NotFoundException

----------


Coding Style
-------------------

\#region을 사용하여 코드를 묶는다.

    public class MainViewModel : ViewModelBase
    {
	    #region Variable
	    
	    // const variables
	    // static variables
	    // readyonly variables
	    // private variables
	    // protected variables
	    // public variables
	    
	    #endregion
		
		#region Property
	
		#region OrderNumber

		private int _orderNumber;
		public int OrderNumber
		{
			Get { return _orderNumber; }
			Set { Set("OrderNumber", ref _orderNumber, value); }
		}

		#endregion

		#region OrderId

		public int OrderId { get; set; }

		#endregion		

		#endregion

		#region Command

		#region SearchOrder

		private ICommand _searchOrderCommand;
		public ICommand SearchOrderCommand
		{
			get{ return _searchOrderCommand ?? (_searchOrderCommand = new RelayCommand<int>(OnSearchOrder, CanSearchOrder)); }
		}

		private void OnSearchOrder(int id)
		{
		}
		
		// command 수행여부 필요시만 작성
		private bool CanSearchOrder(int id)
		{
			return true;
		}

		#endregion
			
		#endregion 

		#region Constructor
		#endregion

		#region Public Method
		#endregion

		#region Protected Method
		#endregion
	
		#region Private Method
		#endregion

		#region Event Handler
		#endregion
    }

주석은 되도록 사용하지 말고 알고리즘과 같이 이해를 돕기 위해 꼭 필요한 경우 사용한다.
> - 사용되지 않는 코드는 주석으로 남기지 말고 제거한다.
> - Git으로 히스토리 파악

UI 관련 문자열은 resource 파일을 사용하여 관리한다.
> 다국어 지원 용이

클래스간 종속성을 최소화한다.
> DI

공통기능에 대한 작업은 AOP를 사용한다.
> Fody

사용하지 않는 using 은 항상 제거한다.
탭을 통해 들여쓰기 하고 크기는 4로 한다.
빈 줄을 통해 영역을 구분한다.
> - 멤버 선언
> - 메소드 간
> - 논리적 로직 구분
괄호는 항상 쌍으로 사용한다.

    if (Valid)
    {
	    ...
    }
데이터 타입은 빌트인 타입을 사용한다.
> Int32 -> int
문자열 병합은 + 연산자 대신 string.format이나 StringBuilder를 사용한다.
IDisposable 구현 객체는 using 블록으로 감싼다.

    using (var sqlConnection = new SqlConnection())
    {
	    ...
    }
if 조건문에 bool 변수가 있다면 true/false와 비교하지 않는다.

	
    if (isValid == true)
    {
	    ...
    }

	if (isValid)
	{
		...
	}
	
if 조건문에서 할당하지 않는다.

----------


WPF
-------------------

MVVM 패턴을 사용한다.
> MvvmLight

뷰와 연결된 cs 파일에 코드를 작성하지 않는다.
> 꼭 필요한 경우 종속성이 발생하지 않게 한다.

뷰 전환은 뷰 전환을 담당하는 클래스를 통해 전환하고 관리한다.
> NavigationService

x:Name을 통한 접근은 절대 사용하지 않는다.
> xaml 내 레이아웃 정리 용도 및 바인딩을 위한 경우는 예외

비즈니스 기능 관련 모듈이 UI 모듈을 참조하지 않도록 한다.
UI 관련 작업은 최대한 xaml을 사용한다.
> cs 코드가 필요한 공통 기능에 대해서는 Behavior나 Control을 작성한다.
> > 종속적이지 않고 일반적인 형식이어야 한다.

공통적인 스타일이나 템플릿은 리소스 파일로 구분하여 작성한다.
> 테마 변경에 유용

뷰모델과 같은 형식의 Design 뷰모델을 작성하여 샘플 데이터를 제공한다.
> Blend 작업시 데이터를 미리 확인할 수 있고 실행전에 미리 테스트 할 수 있다.  
> d:DataContext="{d:DesignInstance IsDesignTimeCreatable=True, Type={x:Type designVm:DesignMainViewModel}}"

----------

### Reference
[1] https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/inside-a-program/coding-conventions  
[2] http://www.dofactory.com/reference/csharp-coding-standards  
[3] http://www.csharpstudy.com/Guide/coding-guide.aspx  
[4] http://se.inf.ethz.ch/old/teaching/ss2007/251-0290-00/project/CSharpCodingStandards.pdf  

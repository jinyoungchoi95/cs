<details>
<summary>싱글톤 패턴</summary>
<div markdown="1">

- 싱글톤 패턴은 생성패턴 중하나이다.
	- 시스템이 어떤 구체 클래스를 사용하는지에 대한 정보를 캡슐화
	- 어떻게 만들고 어떻게 결합하는지에 대한 부분을 가려줌
	- 따라서 객체의 생성과 조합을 캡슐화하여 객체가 생성되거나 변경되어도 시스템 구조에 크게 영향 받지 않도록 한다.
- 클래스의 인스턴스가 하나임을 항상 보장하는 패턴
- logging, thread pool 등 여러 객체를 관리하는 역할의 객체에 주로 사용
- 구현 방법
1. Eager initialization
```java
public class Singleton {

	private static final Singleton instance = new SingleTone();

	private Singleton() {
	}

	public static Singleton getInstance() {
		return instance;
	}
}
```
- 가장 간단한 방법
- 클래스 로딩단에서 instance를 생성함. 다만 클래스 로딩단에서 생성하기 때문에 사용하지 않는 경우 낭비발생
2. static block initialization
```java
public class Singleton {

	private static final Singleton instance;

	static {
		try {
			instance = new Singleton();
		} catch(Exception e) {
			// throw exception
		}
	}

	private Singleton() {
	}

	public static Singleton getInstance() {
		return instance;
	}
}
```
1번 방법과는 다르게 exception 발생 시 처리를 진행할 수 있으나 마찬가지로 클래스 로딩시점에 생기기 때문에 낭비발생

3.  Lazy initialzation
```java
public class Singleton {

	private static Singleton instance;

	private Singleton() {
	}

	public static Singleton getInstance() {
		if (instance == null) {
			instance = new Singleton();
		}
		return instance;
	}
}

```
- 클래스 로딩시점이 아닌 `getInstance()` 호출 시점에 인스턴스가 생성되기 때문에 낭비에서 해결이 된다. 다만 멀티스레드환경에서 여러개의 스레드가 동시에 해당 메소드를 호출하게 되는 경우 인스턴스가 여러개 생성될 수 있음
- 싱글스레드 환경이 보장되는 경우에만 사용해야함 (결국 스프링에선 사용할 수 없음)

4. Thread safe 
```java
public class Singleton {

	private static Singleton instance;

	private Singleton() {
	}

	public static synchronized Singleton getInstance() {
		if (instance == null) {
			instance = new Singleton();
		}
		return instance;
	}
}
```
- `getInstance()`의 호출이 여러 스레드에서 일어나느 것이 문제라면 `synchronized`를 사용하여 메소드에 들어오는 스레드를 하나로 제한걸면 된다.
- 다만 `getInstance()` 호출 시 이미 인스턴스가 호출되었는데도 `synchronized`가 걸리기 때문에 아래와 같이 null인 경우에만 lock을 걸면 된다.
```java
public class Singleton {

	private static Singleton instance;

	private Singleton() {
	}

	public static synchronized Singleton getInstance() {
		if (instance == null) {
			synchronized (Singleton.class) {
				instance = new Singleton();
			}
		}
		return instance;
	}
}
```

5. LazyHolder
```java
public class Singleton {

	private Singleton() {
	}

	public static Singleton getInstance() {
		return LazyHolder.instance;
	}

	private static class LazyHolder {
		private static final Singleton instance = new Singleton();
	}
}
```
- 인스턴스에 대한 생성 자체를 JVM에게 맡기는 방법이다.
- 클래스의 내부 클래스는 클래스 로딩이 될 때 올라가지 않기 때문에 내부 클래스에 숨겨둔 다음 호출될 때 내부 클래스를 로딩하는 방식이다.
- 이때 내부 클래스를 로딩하고 초기화하는 것은 JVM의 영역이며 Thread safe를 보장한다.

6. Enum
```java
public enum Singleton {

	INSTANCE;

	// code
}
```
- enum은 싱글톤임을 항상 보장하기 때문에 해당 방식을 사용할 수 있다.
</div>
</details>

<details>
<summary>팩토리 패턴</summary>
<div markdown="1">
	
- 객체 생성 부분을 떼어내 추상화한 패턴
- 보통은 상위 클래스 하나가 있고 이에 대한 구현체가 여러개 있을 때 상황에 따라 원하는 인스턴스를 리턴해주는 방식이다
- 따라서 인터페이스에 따른 구현체를 두기 편해지기 때문에 추상화에 도움이 된다.
	
</div>
</details>

<details>
<summary>전략 패턴</summary>
<div markdown="1">

- 특정한 행위가 있을때 해당 행위에 대해서 추상화를 한 인터페이스를 두고, 각각의 행위에 대한 전략에 따라 구현체를 두는 디자인 패턴이다. 필요한 상황에 따라서 특정 전략 구현체를 사용하는 방식으로 전략을 수정한다.
- 상태패턴도 마찬가지로 상태에 대한 추상화된 인터페이스를 두고 상태를 계속해서 변경하는 방법이기는 하다. 다만 상태패턴은 현재 상태에서 메소드가 동작했을 때 다음 상태에 대해서 변화할 수 있지만 전략 패턴은 전략 그자체에 대한 행위만을 진행하여 다른 전략으로의 상태변화가 없다.
```java
public interface MoveStrategy {

	boolean move(int value);
}

public class AlwaysMoveStrategy implements MoveStrategy {

	@Override
	public boolean move(int value) {
		return true;
	}
}

public class CarMoveStrategy implements MoveStrategy {

	@Override
	public boolean move(int value) {
		return value >= 4;
	}
}
```

</div>
</details>

<details>
<summary>옵저버 패턴</summary>
<div markdown="1">

- 어떤 객체의 상태 변화를 관찰하는 옵저버를 등록하고, 필요한 상태의 변화가 있을 때마다 옵저버 목록에 있는 옵저버들에게 변화를 통지하는 디자인 패턴
- 자바에서는 아래와 같은 이유로 Observer 인터페이스가 deprecated되었으며 그 이유는 아래와 같다.
	- Observable이 클래스로 정의되어있어 상속받는 구조로 되어있으며, 다른 클래스를 이미 상속받고 있다면 상속받을 수 없어 재사용성에 문제가 있다.
	- Observer의 구현 로직이 instance of로 타입체크를 하며 casting이 필수적이다
	- thread safe하지 않음
```java
public class EventHandler implements PropertyChangeListener {  
  
    @Override  
    public void propertyChange(final PropertyChangeEvent evt) {  
        if (evt.getPropertyName().equals("Domain.name")) {  
            System.out.println("event was handling");  
        }  
    }  
}
```

```java
public class Domain {  
      
    private String name;  
  
    private final PropertyChangeSupport listeners = new PropertyChangeSupport(this);  
  
    public Domain() {  
    }  
  
    public void addPropertyChangeListener(PropertyChangeListener listener) {  
        listeners.addPropertyChangeListener(listener);  
    }  
  
    public void firePropertyChange(String propName, Object oldValue, Object newValue) {  
        listeners.firePropertyChange(propName, oldValue, newValue);  
    }  
  
    public void setName(final String name) {  
        firePropertyChange("Domain.name", this.name, name);  
        this.name = name;  
    }  
}
```

```java
public static void main(String[] args) {  
    EventHandler eventHandler = new EventHandler();  
    Domain domain = new Domain();  
    domain.addPropertyChangeListener(eventHandler);  
  
    domain.setName("도메인 이름 변경");  
}
```

</div>
</details>

<details>
<summary>프록시 패턴과 프록시 서버</summary>
<div markdown="1">

- proxy : 어떤 대상의 기본적인 작업을 가로챌 수 있는 객체
- handler : 프록시 객체의 target동작을 가로채서 정의할 동작들이 정해져 있는 함수
- 따라서 프록시 패턴이란 target에 접근하기 전 흐름을 가져가서 앞단에 정해진 행동을 진행하도록 하는 패턴을 말한다.
- 객체를 보완하는 역할로써 로깅, 캐싱, 보안 등에 활용가능

- 프록시 서버란 클라이언트가 서버에 접속요청을 하기 전 본인이 요청을 가로채서 접속하는 서버이다.
- cors 문제를 해결하기 위한 방법중 하나로 사용된다.
- cors란 Cross-Origin-Resource-Sharing으로 서버에서 자원을 요청할 때 다른 origin으로 요청하는 경우 요청을 로딩하지 못하게 하는 http 기법이다.
- 일반적으로 프론트 요청에 대해서 백엔드의 origin 정보가 다르기 때문에 cors가 발생하는데, 이를 해결하기 위해서 프록시 서버가 백엔드 서버와 같은 origin으로 바꾸어 요청한다.

- 리버스 프록시 서버란 서버에 요청이 들어올 때 해당 요청을 가로채서 처리하는 서버이다.
- 캐싱, https에 대해서 처리를 해줄 수 있으며 백엔드 어플리케이션 서버에 대해서 ip를 노출시키지않고 리버스 프록시 서버에서 요청에 대해서 리버스프록싱해줌으로써 직접적인 ip 접속을 차단할 수 있다.
	
</div>
</details>

<details>
<summary>이터레이터 패턴</summary>
<div markdown="1">

- 여러개의 Collection 자료구조가 있을 때 iterator를 통해 각 자료구조로 접근할 수 있도록 추상화한 방식
- 자바에서는 이미 iterator가 구현체로 존재함

</div>
</details>

<details>
<summary>노출모듈 패턴</summary>
<div markdown="1">

- 즉시 실행 함수를 통해 private, public과 같은 접근 제어자를 만드는 패턴
- js는 전역 범위 내에서 스크립트가 진행되기 때문에 직접 구현해야한다.

</div>
</details>

<details>
<summary>MVC 패턴</summary>
<div markdown="1">

- model, view, controller로 어플리케이션의 통신 프로세스에서 크게 3가지 관심사로 나눈 패턴
- Model
	- 어플리케이션의 데이터에 속함
	- view와 controller가 주고받는 데이터
- View
	- 실제 화면단에서 나타나있는 view 화면 그자체를 의미함. 따로 데이터를 저장하여 본인이 해결하는 것이 아닌 화면 랜더링만 진행하고 필요한 데이터는 model을 통해 노출시킴
- Controller
	- View가 필요한 Model을 만들어내는 역할
	- 어플리케이션 로직을 담당하여 필요한 데이터를 만들어 model에 담아 View에 전달

</div>
</details>

<details>
<summary>MVP 패턴</summary>
<div markdown="1">

- MVC에서 C가 Presenter로 교체된 패턴
- Model에 대한 요청을 View가 하는 것이 아닌 Persenter가 진행하여 View와 Model간의 의존성이 떨어짐

</div>
</details>

<details>
<summary>MVVM </summary>
<div markdown="1">

- Model, View, View Model로 이루어진 패턴
- View Model은 View를 조금 더 추상화한 계층으로 커맨드와 데이터 바인딩을 가지고 있음
- View에 요청이 들어오면 Command 패턴을 통해 View Model에 Action을 전달하는 형태
- View와 Model간의 의존성이 없는 것은 MVP와 동일하며, View와 View Model간의 의존성또한 없어 각각이 독립적인 모듈 개발을 진행할 수 있음

</div>
</details>

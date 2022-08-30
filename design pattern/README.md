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

</div>
</details>

<details>
<summary>이터레이터 패턴</summary>
<div markdown="1">

</div>
</details>

<details>
<summary>노출모듈 패턴</summary>
<div markdown="1">

</div>
</details>

<details>
<summary>MVC 패턴</summary>
<div markdown="1">

</div>
</details>

<details>
<summary>MVP 패턴</summary>
<div markdown="1">

</div>
</details>

<details>
<summary>MVVM </summary>
<div markdown="1">

</div>
</details>

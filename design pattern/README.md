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

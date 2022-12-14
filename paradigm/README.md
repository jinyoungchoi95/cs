<details>
<summary>함수형 프로그래밍 </summary>
<div markdown="1">

- 프로그래밍 패러다임 중 선언형의 대표적인 패러다임은 함수형 프로그래밍이다.
- 무언가 행위자체를 해결하기 위해서 나온 패러다임으로 "프로그램은 함수로 이루어진 것이다."라는 명제가 담겨있다.
- 일반적으로 자바에서는 객체지향 프로그래밍을 다루기 때문에 함수형 프로그래밍에 대한 지원이 늦어졌는데, 자바 8부터 람다식을 지원함으로써 이를 풀어냈었다. 따라서 이 이후로는 특정한 행위에 대해서 함수로 추상화하여 조금더 유연한 구조를 가질 수 있다.
	- 순수함수
		- 출력이 입력에만 의존하는 것
	- 고차 함수
		- 값을 매개변수로 받아 로직을 생성할 수 있는 것
		- 고차함수를 쓰기 위해서는 해당 언어가 일급 객체라는 특징을 가져야하며 특징은 아래와 같다.
			- 변수나 메소드에 함수를 할당할 수 있다
			- 함수 안에 함수를 매개변수로 담을 수 있다
			- 함수가 함수를 반환할 수 있다

</div>
</details>

<details>
<summary>객체지향 프로그래밍 </summary>
<div markdown="1">
	
- 각각의 객체가 특정한 책임을 갖고 역할을 수행하며, 그 각각의 객체가 서로 협력하는 구조를 가지도록 하는 것에 초점을 맞춘 패러다임
- 추상화 + 다형성
	- 복잡한 시스템으로부터 핵심적인 기능 또능 기능을 간추려내는 것
	- 하나의 특정 구현체들로만 이루어진 것이 아닌 공통된 구현체들에 대한 공통의 로직에 대해서 추상화를 하고 이를 통하여 문제해결을 이루어냄
- 캡슐화
	- 객체의 속성과 메소드를 하나로 묶어 외부에 감추어 은닉하는 것
	- 정보 은닉은 아예 접근을 하지 못하는 것이 중점이고, 캡슐화의 주요 목적은 외부에서 객체 내부의 구현 내용을 알지 못하도록 해야한다는 것이다.
	- 내부 구현내용을 알지 못하더라도 객체의 기능을 잘 사용할 수 있도록 하는 것이 중요하다.
- 상속성
	- 상위 클래스의 특성을 하위 클래스가 이어 받아 재사용하도록 하는 것
- 설계원칙
	- 단일 책임 원칙 : 객체는 각각 하나의 책임만을 가져야한다. 이 조건은 상대적인 조건이기는 하지만 단일 책임 원칙을 위반한지 확인하기 위한 방법으로는 한 구현체의 변경점이 다른 구현체에 영향이 많이 가는지 안가는지를 확인하면 된다.
	- 개방 폐쇄원칙 : 확장에는 열려있고 변경에는 닫혀있어야 한다. 
	- 리스코프 치환 원칙 : 객체는 프로그래밍 정확성을 떨어뜨리지 않으면서 하위 인스턴스로 변경가능하여야 한다.
	- 인터페이스 분리 원칙 : 하나의 큰 인터페이스가 아닌 구체적인 여러개의 인터페이스로 분리하여야 한다.
	- 의존 역전 원칙 : 추상화에 의존해야하며 구체화에 의존하면 안된다. 즉, 고수준 모듈이 저수준 모듈의 구현체에 의존해서는 안된다. 인터페이스를 주입받음으로써 구현체에 의존했을 때 발생하는 구현체 변경에서의 확장성이 떨어지는 것을 막을 수 있다.

</div>
</details>

# 다형성이란?

**여러 가지 형태를 가질 수 있는 능력**을 의미하며 자바에서는 한 타입의 참조변수로 여러 타입의 객체를 참조할 수 있도록 함으로써 다형성을 프로그램적으로 구현하였고, **상속**과 깊은 관계가 있다.

이를 좀 더 구체적으로 말하자면, **조상클래스 타입의 참조변수로 자손클래스의 인스턴스를 참조할 수 있도록 하였다**는 것이다.

```java
class Tv {
	boolean power; // 전원상태(on/off)
	int channel;   // 채널

	void power() { power = !power; }
	void channelUp() { ++channel; }
	void channelDown() { --channel; }
}

class CaptionTv extends Tv {
	String text;  // 캡션을 보여주기 위한 문자열
	void caption() { /* 내용생략 */ }
}
```

```java
CaptionTv c = new CaptionTv(); // 모든 맴버들을 사용할 수 있다.
Tv t = new CaptionTv(); // Tv클래스의 맴버들(상속받은 맴버포함)만 사용할 수 있다. text와 caption()은 참조변수 t로 사용이 불가능하다.
```

**둘 다 같은 타입의 인스턴스지만 참조변수의 타입에 따라 사용할 수 있는 맴버의 개수가 달라진다.**

```java
Tv t = new CaptionTv(); // 조상타입의 참조변수로 자손 인스턴스를 참조할 수 있다.
CaptionTv c = new Tv(); // 컴파일 에러
```

**조상타입의 참조변수로 자손타입의 인스턴스를 참조할 수 있다.**

**반대로 자손타입의 참조변수로 조상타입의 인스턴스를 참조할 수는 없다.**

그 이유는 실제 인스턴스인 Tv의 맴버 개수보다 참조변수 c가 사용할 수 있는 맴버 개수가 더 많기 때문에 허용하지 않는다.

# 참조변수의 형변환

참조변수도 **형변환**이 가능하다. 단, 서로 **상속관계**에 있는 클래스사이에서만 가능하다.

기본형 변수의 형변환에서 **작은 자료형**에서 **큰 자료형**의 형변환은 **생략이 가능**하듯이, 참조변수의 형변환에서는 아래와 같이 할 수 있다.

**자손타입 → 조상타입 (Up-casting) : 형변환 생략가능**

**자손타입 ← 조상타입 (Down-casting) : 형변환 생략불가**

```java
class Car {
	String color;
	int door;

	void drive() {                // 운전하는 기능
		System.out.println("drive, Brrrr~");
	}

	void stop() {                 // 멈추는 기능
		System.out.println("stop!!!");
	}
}

class FireEngine extends Car {  // 소방차
	void water() {                // 물 뿌리는 기능
		System.out.println("water!!!");
	}
}

class Ambulance extends Car {   // 앰뷸런스
	void siren() {                // 사이렌을 울리는 기능
		System.out.println("siren~~~");
	}
}
```

```java
class CastingTest1 {
	Car car = null;
	FireEngine fe = new FireEngine();
	FireEngine fe2 = null;
	
	fe.water();
	car = fe;                 // car = (Car)fe;에서 형변환 생략됨. 업캐스팅
//  car.water();            // 컴파일 에러!!! Car타입의 참조변수로는 water()를 호출할 수 없다.
	fe2 = (FireEngine)car;    // 형변환 생략불가. 다운캐스팅
	fe2.water();
}
```

형변환은 참조변수의 타입을 변환하는 것이지 인스턴스를 변환하는 것은 아니기 때문에 **참조변수의 형변환은 인스턴스에 아무런 영향을 미치지 않는다.**

단지 **참조변수의 형변환**을 통해서, 참조하고 있는 인스턴스에서 사용할 수 있는 **맴버의 범위(개수)를** 조절하는 것뿐이다.

```java
class CastingTest2 {
	public static void main(String[] args) {
		Car car = new Car();
		Car car2 = null;
		FireEngine fe = null;

		car.drive();
		fe = (FireEngine)car;  // 컴파일은 OK. 실행 시 에러가 발생
		fe.drive();
		car2 = fe;
		car2.drive();
	}
}
```

서로 상속관계에 있는 클래스 타입의 참조변수간의 형변환은 **양방향**으로 자유롭게 수행될 수 있으나, 참조변수가 참조하고 있는 인스턴스의 자손타입으로 형변환을 하는 것은 허용되지 않는다. **참조변수가 가리키는 인스턴스의 타입이 무엇인지 확인하는 것이 중요하다.**

# instanceof연산자

참조변수가 참조하고 있는 인스턴스의 실제 타입을 알아보기 위해 **instanceof연산자**를 사용한다. 주로 **조건문**에 사용되며, instanceof의 왼쪽에는 **참조변수**를 오른쪽에는 **타입(클래스명)이** 피연산자로 위치하고 **연산결과**로 true와 false을 반환한다.

instanceof를 이용한 연산결과로 **true**를 얻었다는 것은 참조변수가 검사한 타입으로 **형변환이 가능하다**는 것을 뜻한다.

```java
class InstanceofTest {
	public static void main(String[] args) {
		FireEngine fe = new FireEngine();

		if(fe instanceof FireEngine) {
			System.out.printlf("This is a FireEngine instance.");
		}

		if(fe instanceof Car) {
			System.out.printlf("This is a Car instance.");
		}

		if(fe instanceof Object) {
			System.out.printlf("This is a Object instance.");
		}

		System.out.println(fe.getClass().getName()); // 클래스의 이름을 출력
	}
}
class Car {}
class FireEngine extends Car {}

/*
[실행결과]
This is a FireEngine instance.
This is a Car instance.
This is a Object instance.
FireEngine
*/
```

위에 예제를 요약하면, 실제 인스턴스와 같은 타입의 instanceof연산 이외의 **조상타입**의 instanceof연산에도 true를 결과로 얻으며, true라는 것은 검사한 타입으로 **형변환**이 가능하다는 것을 뜻한다.

# 참조변수와 인스턴스의 연결

맴버변수가 조상 클래스와 자손 클래스에 **중복으로 정의된 경우**, 조상타입의 참조변수를 사용했을 때는 조상 클래스에 선언된 맴버변수가 사용되고, 자손타입의 참조변수를 사용했을 때는 자손 클래스에 선언된 맴버변수가 사용된다.

**메서드의 경우** 조상 클래스의 메서드를 자손 클래스에서 오버라이딩한 경우에도 참조변수의 타입에 관계없이 **항상 실제 인스턴스의 메서드(오버라이딩된 메서드)가 호출된다.**

```java
class BindingTest {
	public static void main(String[] args) {
		Parent p = new Child();
		Child c = new Child();

		System.out.println("p.x = " + p.x);
		p.method();

		System.out.println("c.x = " + c.x);
		c.method();
	}
}

class Parent {
	int x = 100;

	void method() {
		System.out.println("Parent Method");
	}
}

class Child extends Parent {
	int x = 200;

	void method() {
		System.out.println("Child Method");
	}
}

/*
[실행결과]
p.x = 100
Child Method
c.x = 200
Child Method
*/
```

# 매개변수의 다형성

참조변수의 다형적인 특징은 **메서드의 매개변수**에도 적용된다.

```java
class Product {
	int price;         // 제품의 가격
	int bonusPoint;    // 제품구매 시 제공하는 보너스점수

	Product(int price) {
		this.price = price;
		bonusPoint = (int)(price/10.0);  // 보너스점수는 제품가격의 10%
	}
}

class Tv extends Product {
	Tv() {
		// 조상클래스의 생성자 Product(int price)를 호출한다.
		super(100);  // Tv의 가격을 100만원으로 한다.
	}

	public String toString() {  // Object클래스의 toString()을 오버라이딩한다.
		return "Tv";
	}
}

class Computer extends Product {
	Computer() {
		super(200);
	}

	public String toString() {
		return "Computer";
	}
}

class Buyer {               // 고객, 물건을 사는 사람
	int money = 1000;         // 소유금액
	int bonusPoint = 0;       // 보너스점수

	void buy(Product p) {
		if(money < p.price) {
			System.out.println("잔액이 부족하여 물건을 살 수 없습니다.");
			return;
		}

		money -= p.price;             // 가진 돈에서 구입한 제품의 가격을 뺸다.
		bonusPoint += p.bonusPoint;   // 제품의 보너스점수를 추가한다.
		System.out.println(p + "을/를 구입하셨습니다.");
	}
}

class PolyArgumentTest {
	public static void main(String[] args) {
		Buyer b = new Buyer();

		b.buy(new Tv());
		b.buy(new Computer());

		System.out.println("현재 남은 돈은 " + b.money + "만원입니다.");
		System.out.println("현재 보너스점수는 " + b.bonusPoint + "점입니다.");
	}
}

/*
[실행결과]
Tv을/를 구입하셨습니다.
Computer을/를 구입하셨습니다.
현재 남은 돈은 700만원입니다.
현재 보너스점수는 30점입니다.
*/
```

매개변수의 다형성을 적용한 buy메서드의 매개변수가 Product타입의 참조변수라는 것은 메서드의 매개변수로 Product클래스의 자손타입 참조변수면 어느 것이나 매개변수로 받아들일 수 있다는 뜻이다.

# 여러 종류의 객체를 배열로 다루기

조상타입의 참조변수 배열을 사용하면, 공통의 조상을 가진 서로 다른 종류의 객체를 **배열로 묶어서** 다룰 수 있다.

```java
Product p[] = new Product[3];
p[0] = new Tv();
p[1] = new Computer();
p[2] = new Audio();
```

Product배열의 크기가 3으로 배열의 크기가 고정되어있다. 그렇다고 해서 배열의 크기를 무조건 크게 설정할 수만도 없는 일이다.

이런 경우, **Vector클래스**를 사용하면 된다. Vector클래스는 동적으로 크기가 관리되는 객체배열이다.

# 출처

자바의 정석

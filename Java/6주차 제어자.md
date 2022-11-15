# 제어자란?

제어자는 클래스, 변수 또는 메서드의 선언부에 함께 사용되어 **부가적인 의미를 부여**한다.

## 제어자의 종류

1. 접근제어자
    
    public, protected, default, private
    
2. 그 외
    
    static, final, abstract, native, transient, synchronized, volatile, strictfp
    

제어자는 하나의 대상에 대해 **여러 제어자를 조합**하여 사용하는 것이 가능하다.

단, **접근 제어자**는 한 번에 네 가지 중 **하나만 선택**해서 사용할 수 있다.

# static - 클래스의, 공통적인

static은 **클래스의** 또는 **공통적인**의 의미를 가지고 있다. 클래스변수(static맴버변수)는 인스턴스에 관계없이 **같은 값**을 갖는다.

## static이 사용될 수 있는 곳

맴버변수, 메서드, 초기화 블럭

```java
class StaticTest {
	static int width = 200;          // 클래스 변수(static변수)
	static int height = 120;         // 클래스 변수(static변수)

	static {                         // 클래스 초기화 블럭
		// static변수의 복잡한 초기화 수행
	}

	static int max(int a, int b) {   // 클래스 메서드(static 메서드)
		return a > b ? a : b;
	}
}
```

# final - 마지막의, 변경될 수 없는

final은 **마지막의** 또는 **변경될 수 없는**의 의미를 가지고 있으며 거의 모든 대상에 사용될 수 있다.

**변수**에 사용되면 값을 변경할 수 없는 **상수**가 되며, **메서드**에 사용되면 **오버라이딩**을 할 수 없게 되고 **클래스**에 사용되면 자신을 확장하는 **자손클래스**를 정의하지 못하게 된다.

## final이 사용될 수 있는 곳

클래스, 메서드, 맴버변수, 지역변수

```java
final class FinalTest {      // 조상이 될 수 없는 클래스
	final int MAX_SIZE = 10;   // 값을 변경할 수 없는 맴버변수(상수)

	final int getMaxSize() {   // 오버라이딩할 수 없는 메서드(변경불가)
		final int LV = MAX_SIZE; // 값을 변경할 수 없는 지역변수(상수)
		return MAX_SIZE;
	}
}
```

# abstract - 추상의, 미완성의

abstract는 **미완성**의 의미를 가지고 있다. 메서드의 선언부만 작성하고 실제 수행내용은 구현하지 않은 추상 메서드를 선언하는데 사용된다.

## abstract가 사용될 수 있는 곳

클래스, 메서드

```java
abstract class AbstractTest {  // 추상 클래스(추상 메서드를 포함한 클래스)
	abstract void move();        // 추상 메서드(구현부가 없는 메서드)
}
```

# 접근 제어자(access modifier)

접근 제어자는 **맴버** 또는 **클래스**에 사용되고 외부에서 접근하지 못하도록 **제한하는 역할**을 한다.

## 접근 제어자가 사용될 수 있는 곳

클래스, 맴버변수, 메서드, 생성자

| public | 접근 제한이 전혀 없다. (전체) |
| --- | --- |
| protected | 같은 패키지 내에서, 다른 패키지의 자손클래스에서 접근이 가능하다. |
| default | 같은 패키지 내에서만 접근이 가능하다. |
| private | 같은 클래스 내에서만 접근이 가능하다. |

## 대상에 따라 사용할 수 있는 접근 제어자

| 대상 | 사용가능한 접근 제어자 |
| --- | --- |
| 클래스 | public, (default) |
| 메서드, 맴버변수 | public, protected, (default), private |
| 지역변수 | 없음 |

## 접근 제어자를 이용한 캡슐화

접근 제어자를 사용하는 이유

1. 외부로부터 데이터를 보호하기 위해서
2. 외부에는 불필요한, 내부적으로만 사용되는 부분을 감추기 위해서

만일 메서드 하나를 변경해야 한다고 가정했을 때, 이 메서드의 접근제어자가 **public**이라면, 메서드를 변경한 후에 오류가 없는지 테스트해야하는 범위가 넓다. 그러나 **default**라면 **패키지 내부만** 확인해보면 되고 **private**이면 **클래스 하나만** 살펴보면 된다.

**이처럼 접근 제어자를 적절히 선택해서 접근 범위를 최소화하도록 노력해야 한다.**

```java
class Time {
	// 접근 제어자를 private으로 하여 외부에서 직접 접근하지 못하도록 한다.
	private int hour;

	public int getHour() {
		return hour;
	}

	public void setHour(int hour) {
		if(hour < 0 || hour > 23) return;
		this.hour = hour;
	}
}
```

## 생성자의 접근 제어자

생성자에 접근 제어자를 사용함으로써 인스턴스의 생성을 제한할 수 있다. 보통 생성자의 접근 제어자는 클래스의 접근 제어자와 같지만, 다르게 지정할 수도 있다.

```java
final class Singleton {
	private static Singleton s = new Singleton(); // getInstance()에서 사용될 수 있도록 인스턴스가 미리 생성되어야 하므로 static이어야 한다.

	private Singleton() {
		// ...
	}

	public static Singleton getInstance() {
		if(s == null) {
			s = new Singleton();
		}
		return s;
	}
	// ...
}

class SingletonText {
	public static void main(String[] args) {
//		Singleton s = new Singleton(); // 에러. private 이라 인스턴스를 생성할 수 없다.
		Singleton s = Singleton.getInstance();
	}
}
```

# 제어자(modifier)의 조합

## 대상에 따라 사용할 수 있는 제어자

| 대상 | 사용가능한 제어자 |
| --- | --- |
| 클래스 | public, (default), final, abstract |
| 메서드 | 모든 접근 제어자, final, abstract, static |
| 맴버변수 | 모든 접근 제어자, final, static |
| 지역변수 | final |

## 제어자를 조합해서 사용할 때 주의해야 할 사항

1. **메서드**에 **static**과 **abstract**를 함께 사용할 수 없다.
    
    static메서드는 몸통이 있는 메서드에만 사용할 수 있기 때문이다.
    
2. **클래스**에 **abstract**와 **final**을 동시에 사용할 수 없다.
    
    클래스에 사용되는 final은 클래스를 확장할 수 없다는 의미이고, abstract는 상속을 통해서 완성되어야 한다는 의미이므로 서로 모순되기 때문이다.
    
3. **abstract메서드**의 접근 제어자가 **private**일 수 없다.
    
    abstract메서드는 자손클래스에서 구현해주어야 하는데 접근 제어자가 private이면, 자손클래스에서 접근할 수 없기 때문이다.
    
4. **메서드**에 **private**과 **final**을 같이 사용할 필요는 없다.
    
    접근 제어자가 private인 메서드는 오버라이딩될 수 없기 때문이다. 이 둘 중 하나만 사용해도 의미가 충분하다.
    

# 출처

자바의 정석

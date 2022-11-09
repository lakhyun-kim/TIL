# 패키지(package)

패키지란, **클래스의 묶음**이다.

- 하나의 소스파일에는 첫 번째 문장으로 **단 한 번의 패키지 선언만을 허용**한다.
- 모든 클래스는 **반드시 하나의 패키지**에 속해야 한다.
- 패키지는 **점(.)을 구분자**로 하여 **계층구조**로 구성할 수 있다.
- 패키지는 물리적으로 클래스 파일(.class)을 포함하는 **하나의 디렉토리**이다.

# 패키지의 선언

클래스나 인터페이스의 소스파일(.java)에 다음과 같이 한 줄만 적어주면 된다.

```java
package 패키지명;
```

패키지를 선언하지 않고도 아무런 문제가 없다. 그 이유는 **이름없는 패키지(unnamed package)** 때문이다.

# import문

클래스의 코드를 작성하기 전에 import문으로 사용하고자 하는 클래스의 패키지를 미리 명시해주면 소스코드에 사용되는 클래스이름에서 패키지명은 생략할 수 있다.

컴파일 시에 컴파일러는 import문을 통해 소스파일에 사용된 클래스들의 패키지를 알아 낸 다음, 모든 클래스이름 앞에 패키지명을 붙여 준다.

# import문의 선언

package문 다음에 클래스 선언문 이전에 위치해야 한다.

```java
import 패키지명.클래스명;
또는
import 패키지명.*;
```

**System**과 **String**같은 **java.lang패키지**는 매우 빈번히 사용되는 중요한 클래스들이 속한 패키지이기 때문에 **모든 소스파일**에는 묵시적으로 **java.lang패키지**가 import문이 선언되어 있다.

# static import문

static import문을 사용하면 static맴버를 호출할 때 클래스 이름을 생략할 수 있다. 특정 클래스의 static맴버를 자주 사용할 때 편리하다.

```java
import static java.lang.Integer.*;    // Integer클래스의 모든 static메서드
import static java.lang.Math.random;  // Math.random()만. 괄호 안붙임.
import static java.lang.System.out;   // System.out을 out만으로 참조가능
```

```java
import static java.lang.System.out;
import static java.lang.Math.*;

class StaticImportEx1 {
	public static void main(String[] args) {
		// System.out.println(Math.random());
		out.println(random());

		// System.out.println("Math.PI : " + Math.PI);
		out.println("Math.PI : " + PI);
	}
}
```

# 출처

자바의 정석

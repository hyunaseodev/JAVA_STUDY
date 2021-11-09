# public static final

인터페이스 변수 선언시 public static final 을 제어자로 지정하는 이유가 궁금했다.

**public**, **final**과 **static**의 특징을 하나씩 살펴보면,

**public**, 모든 곳에서 접근이 가능하다.

**final**, 리터럴 변경 불가능하다.

**static**, 인터페이스는 객체 생성이 불가능하므로 변수를 사용하려면 static키워드를 붙여 static 메모리 영역에 로딩 후 생성되어야 한다.

인터페이스에 선언된 변수는 모든 곳에서 접근이 가능하지만(public) 값 변경은 불가능하다(final). 또한 변수를 사용하려면 static 메모리 영역에 로딩 후 생성되어야 한다(static).

참고:

[https://velog.io/@foeverna/Java-42인터페이스-Interface](https://velog.io/@foeverna/Java-42%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4-Interface)
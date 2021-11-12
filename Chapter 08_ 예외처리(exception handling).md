# Chapter 08. 예외처리(exception handling)

## 1. 예외처리

### 프로그램 오류

- 종류
    - 컴파일 에러: 컴파일 시에 발생하는 에러
    - 런타임 에러: 실행 시에 발생하는 에러
        - 에러(error): 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
                           (메모리 부족 - OutOfMemoryError, 스택오버플로우 - StackOverflowError)
        - 예외(exception): 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류
    - 논리적 에러: 실행은 되지만, 의도와 다르게 동작하는 것
- 컴파일러가 소스코드의 기본적인 사항은 컴파일 시에 모두 걸러줄 수는 있지만, 실행도중에 발생할 수 있는 잠재적인 오류까지 검사할 수 없기 때문에 컴파일은 잘되었어도 실행 중에 에러에 의해서 잘못된 결과를 얻거나 프로그램이 비정상적으로 종료될 수 있다.

### 예외처리하기 - try-catch문

- 발생한 예외를 처리하지 못하면, 프로그램은 비정상적으로 종료되며, 처리되지 못한 예외는 JVM의 '예외처리기(UncaughtExceptionHandler)'가 받아서 예외의 원인을 화면에 출력한다.

### 예외의 발생과 catch블럭

- 모든 예외 클래스는 Exception클래스의 자손이므로, catch블럭의 괄호( )에 Exception클래스 타입의 참조변수를 선언해 놓으면 어떤 종류의 예외가 발생하더라도 이 catch블럭에 의해서 처리된다.

### printStackTrace()와 getMessage()

- 예외가 발생했을 때 생성되는 예외 클래스의 인스턴스에는 발생한 예외에 대한 정보가 담겨 있으며, printStackTrace()와 getMessage()를 통해서 이 정보들을 얻을 수 있다.
    - printStackTrace()
    : 예외발생 당시의 호출스택(Call Stack)에 있었던 메서드의 정보와 예외 메시지를 화면에 출력한다.
    - getMessage()
        
        : 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.
        

### 멀티 catch블럭

- JDK1.7부터 여러 catch블럭을 '|'기호를 이용해서, 하나의 catch블럭으로 합칠 수 있게 되었으며, 이를 '멀티 catch블럭'이라 한다.
- 중복된 코드를 줄일 수 있으며 예외 클래스의 개수에는 제한이 없다.

### 예외 발생시키기

- 키워드 throw를 사용해서 프로그래머가 고의로 예외를 발생시킬 수 있다.
    1. 먼저, 연산자 new를 이용해서 발생시키려는 예외 클래스의 객체를 만든 다음
    Exception e = new Exception("고의로 발생시켰음");
    2. 키워드 throw를 이용해서 예외를 발생시킨다.
        
        throw e;
        
- Exception인스턴스를 생성할 때, 생성자에 String을 넣어주면, 이 String이 Exception인스턴스에 메시지로 저장된다. 이 메시지는 getMessage()를 이용해서 얻을 수 있다.

### 메서드에 예외 선언하기

- 메서드의 선언부에 키워드 throws를 사용해서 메서드 내에서 발생할 수 있는 예외를 적으면 된다. 예외가 여러 개일 경우에는 쉼표(,)로 구분한다.

```jsx
void method() throws Exception1, Exception2, ... ExceptionN {
	// 메서드의 내용
}
```

- 메서드를 작성할 때 메서드 내에서 발생할 가능성이 있는 예외를 메서드의 선언부에 명시하여 이 메서드를 사용하는 쪽에서는 이에 대한 처리를 하도록 강요하기 때문에, 프로그래머들의 짐을 덜어주며 견고한 프로그램 코드를 작성할 수 있도록 도와준다.

### finally블럭

- finally블럭은 예외의 발생여부 상관없이 실행되어야할 코드를 포함시킬 목적으로 사용된다.

### 자동 자원 반환 - try-with-resources문

- JDK1.7부터 try-with-resources문이라는 try-catch문의 변형이 새로 추가되었다.
- 입출력(I/O)과 관련된 클래스를 사용할 때 유용하다.
- 사용됐던 자원을 반환하기 위해서 입출력에 사용된 클래스 중 사용한 후에 꼭 닫아 줘야 하는 것들이 있고 finally 블럭 안에 close()를 넣는다.
문제는 close()가 예외를 발생 시킬 수 있고 이를 해결하기 위해 finally 블럭 안에 try-catch문을 추가해서 close()에서 발생할 수 있는 예외를 처리할 수 있도록 변경한다.
하지만 코드가 복잡해지면 try블럭과 finally블럭에서 모두 예외가 발생하면, try블럭의 예외는 무시된다.
이점을 개선하기 위해 try - with - resources문이 추가된 것이다.
- try - with - resources문의 괄호()안에 객체를 생성하는 문장을 넣으면, 이 객체는 따로 close()를 호출하지 않아도 try블럭을 벗어나는 순간 자동적으로 close()가 호출된다. 그 다음에 catch블럭 또는 finally블럭이 수행된다.

```java
try (FileInputStream fis = new FileInputStream("score.dat");
		DataInputStream dis = new DataInputStream(fis)) {
	while(true) {
		...
	}
} catch (EOFException e) {
	...
} catch (EOFException e) {
	...
}
```
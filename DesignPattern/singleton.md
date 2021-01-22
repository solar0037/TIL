# 싱글턴

- [싱글턴 패턴 - 위키백과](https://ko.wikipedia.org/wiki/%EC%8B%B1%EA%B8%80%ED%84%B4_%ED%8C%A8%ED%84%B4)
- [07-3 정적 변수와 메소드 (static) - 점프 투 자바](https://wikidocs.net/228#singleton-pattern)
- [여러가지 싱글톤(singleton) 구현방법 - Python Snippets - 파이썬 조각 코드 모음집](https://wikidocs.net/3693)
- [깊이 있는 Singleton](https://ssoco.tistory.com/65)

> - 싱글턴
> - 언어별 구현
>   - Java
>   - Python
> - 문제점, 주의사항

## 싱글턴

- 객체를 하나만 생성하도록 강제
- 이후 객체를 생성하는 대신 기존의 객체를 반환

## 언어별 구현

### Java

싱글스레드

```Java
class Singleton {
    private static Singleton one;

    private Singleton() {}

    public static Singleton getInstance() {
        if (one == null) {
            one = new Singleton();
        }
        return one;
    }
}

public class Main {
    public static void main(String[] args) {
        Singleton st1 = Singleton.getInstance();  // return new Singleton();
        Singleton st2 = Singleton.getInstance();  // return one; -> st1
        System.out.println(st1 == st2);
    }
}
```

- 생성자에 private 키워드를 사용해 new 키워드로 생성 / 상속 불가
- 대신 getInstance() 메서드 사용

### Python

-

## 문제점, 주의사항

- 상태가 있는 객체를 공유하면 X (멀티스레드 환경에서 위험)
- 클래스 간 의존성 증가, 테스트 힘들어짐

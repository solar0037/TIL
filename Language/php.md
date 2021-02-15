# PHP

> - 개요
> - ==(느슨한 비교)
> - strcmp()
> - md5()

## 개요

- PHP를 공부하는 건 아니고, 웹해킹을 공부하면서 알게 된 PHP의 취약점들

## ==(느슨한 비교)

- 쓰지 말자(JS에서 그러듯이)
- 자동으로 타입을 바꾸어 비교
- 결과를 예측할 수 없는 마법의 연산자

```PHP
echo(0 == NULL);  // 1
echo(1 == '1');  // 1
```

## strcmp()

```PHP
if (strcmp($_POST['password'], $password) == 0) {
    echo "Sucess!";
    exit();
} else {
    echo "Wrong password..";
}
```

- [[PHP] strcmp 취약점을 이용한 인증 우회](https://hackability.kr/entry/PHP-strcmp-%EC%B7%A8%EC%95%BD%EC%A0%90%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%9D%B8%EC%A6%9D-%EC%9A%B0%ED%9A%8C)
- PHP 5.3에서 Array와 String을 비교할 시 `NULL` 반환
- `NULL == 0`이 되어 참
- `GET` 요청에서 query string으로 `[URL]&password[]=asdf`같이 적어주기
- `POST` 요청에서는 `curl [URL] -d "password[]=asdf"`같이 적어주기

## md5()

```PHP
"select * from users where password='".md5($pw, true)."'"
```

- [MD5 common bypass in PHP](https://www.programmersought.com/article/21156493602/)
- `$pw` = ffifdyop
- `md5($pw)` = 276f722736c95d99e921722cf9ed621c (16진법 인코딩)
- 27 -> '
- 6f -> o
- ...
- 276f722736c95d99e921722cf9ed621c -> 'or'6É]é!r,ùíb
- `select * from users where password=''or'6... 이 되어 우회 가능

# JWT Алгоритмы: варианты использования

* **Integrity**: Никто не изменил ваши данные 
* **Authenticity**: Вы можете проверить источник данных
* **Non-repudiation \(from authority\)**: Не только вы можете проверить источник данных
* **Confidentiality**: Данные сохранены в секрете от неавторизованных сторон

### **Классы алгоритмов** 

| Name | Integrity | Confidentiality | Authenticity | Resilience |
| :--- | :---: | :---: | :---: | :---: |
| HMAC | ✅ | 🚫 | ✅ | 🚫 |
| Authenticated encryption | ✅ | ✅ | ✅ | 🚫 |
| Digital signatures | ✅ | 🚫 | ✅ | ✅ |

### **Hash-based Message Authentication Code \(HMAC\)**

**Алгоритмы:**

* HS256
* HS384
* HS512

**Варианты использования:**

* Stateless сессия, хранимая в куки-файлах браузера
* Коды проверки электронной почты
* ID сессий с возможностью обнаруживать истекшие или неправильные ID
* Токены, где издатель и конечный пользователь являются одним лицом

Пример JWT для stateless куки сессии:

```text
{
  "sub"      : "user-12345",
  "email"    : "alice@wonderland.net",
  "login_ip" : "172.16.254.1", 
  "exp"      : 1471102267
}
```

### Digital signatures

**Алгоритмы:**

[RSA подпись с PKCS \#1 и SHA-2:](https://tools.ietf.org/html/rfc7518#section-3.3)

* RS256
* RS384
* RS512

[RSA PSS подпись с SHA-2](https://tools.ietf.org/html/rfc7518#section-3.5):

* PS256
* PS384
* PS512

[EC DSA подпись с SHA-2](https://tools.ietf.org/html/rfc7518#section-3.4):

* ES256
* ES384
* ES512

[Edwards-curve DSA подпись с SHA-2](https://tools.ietf.org/html/rfc8037#section-3.1):

* Ed25519
* Ed448

**Варианты использования:**

* ID токены \(OpenID Connect\)
* Автономные токены доступа \(OAuth 2.0\)
* Передача политик безопасности между доменами
* Данные, целостность и подлинность которых требуется подтверждать другими сторонами

### Authenticated encryption

**Алгоритмы:**

Шифрование публичного / приватного ключа:

* RSA:
  * [RSA OAEP](https://tools.ietf.org/html/rfc7518#section-4.3):
    * RSA-OAEP
    * RSA-OAEP-256
  * [RSA PKCS \#1](https://tools.ietf.org/html/rfc7518#section-4.2):
    * ~~RSA1\_5~~ \(устарел в силу [уязвимости](https://en.wikipedia.org/wiki/Adaptive_chosen-ciphertext_attack)\)
* EC:
  * [ECDH-ES](https://tools.ietf.org/html/rfc7518#section-4.6) с EC или [OKP](https://tools.ietf.org/html/rfc8037#section-3.2) ключем:
    * ECDH-ES
    * ECDH-ES+A128KW
    * ECDH-ES+A192KW
    * ECDH-ES+A256KW

Шифрование секретного \(общего\) ключа:

* [AES прямое шифрование](https://tools.ietf.org/html/rfc7518#section-4.5):
  * dir
* [AES обертка ключа](https://tools.ietf.org/html/rfc7518#section-4.4):
  * A128KW
  * A192KW
  * A256KW
* [AES GCM шифрование ключа](https://tools.ietf.org/html/rfc7518#section-4.7):
  * A128GCMKW
  * A192GCMKW
  * A256GCMKW

Шифрование с использованием пароля:

* [PBES2](https://tools.ietf.org/html/rfc7518#section-4.8):
  * PBES2-HS256+A128KW
  * PBES2-HS384+A192KW
  * PBES2-HS512+A256KW

**Варианты использования:**

* Подписанные и зашифрованные ID токены \(OpenID Connect\)
* Подписанные и зашифрованные автономные токены доступа \(OAuth 2.0\)
* Зашифрованные документы и данные с проверкой целостности и подлинности

Пример JWE-заголовка, данные будут зашифрованы при помощи 128-битного AES алгоритма в режиме GCM, а AES CEK зашифрован с помощью RSA \(RSA OAEP\):

```text
{
  "alg" : "RSA-OAEP",
  "enc" : "A128GCM"
}
```


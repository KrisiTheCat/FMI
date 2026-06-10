# Web Security & Network Protocols

## Теми за изпита:
* Основи на сесиите в PHP ($_SESSION, session_start(), session_destroy())
* Роля на Session ID (PHPSESSID) и бисквитките (Cookies)
* Правилно пренасочване в PHP (header() + exit;) и потискане на грешки с @
* Session Hijacking (Кражба на сесия)
* Session Fixation (Засаждане на сесия) и защита чрез session_regenerate_id(true)
* Сигурно съхранение на пароли чрез password_hash() и password_verify()
* Rainbow Tables (Дъгови таблици), Reduce функция и дълбочина на веригата
* Salt (Сол) и Pepper (Пипер) при съхранение на пароли
* Key Derivation Functions (KDF) - PBKDF2 с итерации
* Argon2 (memory-hard функция за защита от GPU brute-force атаки)
* Асиметрично криптиране (RSA) - публичен/частен ключ, математически модел и операции
* Симетрично криптиране (AES-256)
* Режими на работа на блок шифри (Block Cipher Modes - ECB, CBC, GCM)
* Значение на IV (Initialization Vector) при шифрирането
* Hash Length Extension Attack (при Merkle–Damgård функции)
* Side-channel / Timing attacks (атаки по време) и сравнение за постоянно време
* MAC и HMAC (автентикация на съобщения и защита от length extension)
* Принципи на TLS (Transport Layer Security) за защита на данни в движение
* Ключов обмен (Key Exchange) чрез Diffie-Hellman (DH) и ECDHE (над елиптични криви)
* Perfect Forward Secrecy (PFS)
* Инфраструктура на доверие - Certificate Authorities (CA) и CAA записи
* Man-in-the-Middle (MITM) атаки по мрежата
* SSLStrip (атака за downgrade от HTTPS към HTTP)
* HSTS (HTTP Strict Transport Security) заглавка
* SOP (Same-Origin Policy) за браузърна изолация
* CORS (Cross-Origin Resource Sharing) и Preflight (OPTIONS) заявки
* Cross-Site Scripting (XSS) - Reflected (непостоянна) XSS
* Cross-Site Scripting (XSS) - Stored (постоянна) XSS
* Cross-Site Scripting (XSS) - DOM-based XSS
* Защита срещу XSS чрез CSP (Content Security Policy) заглавка и контекстно ескейпване
* CSRF (Cross-Site Request Forgery) и защита с Anti-CSRF токени и SameSite флагове
* Clickjacking (UI Redressing) и защита с X-Frame-Options / CSP frame-ancestors
* SQL Injection (SQLi) - MySQL инжекции (In-band, Error-based, Blind)
* Защита от SQLi чрез Prepared Statements (параметризирани заявки)
* SSRF (Server-Side Request Forgery)
* Автентикация с Токени вместо парола
* Атаки срещу JWT (уязвимост alg: none и brute-force на слаб секрет)

## Session Management & Cookies

### Основна концепция
* **[`$_SESSION`](https://www.php.net/manual/en/book.session.php)**: асоциативен масив в PHP, съдържащ сесийна информация и локални променливи.
* **[`Session ID` (PHPSESSID)](https://en.wikipedia.org/wiki/Session_(computer_science)#Session_ID)**: уникален идентификатор, изпращан от браузъра за разпознаване на сесията на сървъра.
* Манипулация на сесия:
  * `session_start();`
  * запис: `$_SESSION['user']='ivan';`
  * четене: `$u = $_SESSION['user'];`
  * изчистване: `session_unset(); session_destroy();`

### Управление на грешки и пренасочване
* **Suppress warnings**: `@function_call()` (избягва хиджак на логове, но не премахва истинския проблем).
* **Пренасочване**: `header('Location: /nextpage.php'); exit;` (трябва да се извиква преди изход).
* **Прехвърляне на данни през сесия**: за чувствителни променливи (не GET/POST), например `$_SESSION['cart']=$cart;`.

### Атаки срещу сесиите

* **[`Session hijacking`](https://en.wikipedia.org/wiki/Session_hijacking)**: кражба/компрометиране на идентификатора на сесията.
* **[`Session fixation`](https://en.wikipedia.org/wiki/Session_fixation)**: нападателят налага предварително известен Session ID на браузъра на жертвата.
    1. **Scenario A**: Атакуващият предоставя ID, потребителят се логва с него, и сесията става достъпна за хакера.
    2. **Scenario B**: Атакуващият принуждава потребителя да ползва неговия акаунт.
* **Решение**: `session_regenerate_id(true)` при login/logout/sensitive actions.

---

## TLS (Transport Layer Security) Protocol

### Кратки дефиниции (1-2 изречения)
* **[RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem))**: Ассиметричен (публичен-ключов) алгоритъм за криптиране и подписване.
* **[AES-256](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)**: Симетричен блоков шифър, стандартен за TLS/HTTPS.
* **[SHA-256](https://en.wikipedia.org/wiki/SHA-2)**: Криптографска хеш-функция, използваща 256-битов изход.
* **[Rainbow table](https://en.wikipedia.org/wiki/Rainbow_table)**: Претаблицирани хешове за бърза атака срещу неподсолени пароли.
* **[Diffie–Hellman](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange)**: Протокол за сигурен обмен на ключове.
* **[ECDHE](https://en.wikipedia.org/wiki/Elliptic-curve_Diffie%E2%80%93Hellman)**: Diffie–Hellman над елиптични криви.
* **[HSTS](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security)**: Заглавна политика на браузъра за задължително HTTPS.
* **[MITM](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)**: Атака, при която нападателят прехваща комуникация между две страни.
* **[MAC](https://en.wikipedia.org/wiki/Message_authentication_code)**: код за автентикация на съобщение.
* **[HMAC](https://en.wikipedia.org/wiki/HMAC)**: MAC, базиран на хеш-функция, устойчив на атаките length extension.
* **[PBKDF2](https://en.wikipedia.org/wiki/PBKDF2)**: Функция за извличане на ключ от парола с множество итерации.
* **[Argon2](https://en.wikipedia.org/wiki/Argon2)**: Модерен KDF, устойчив на GPU и паметни атаки.

### RSA (пример)
* Ключове:
  * публичен: $(e, n)$
  * частен: $(d, n)$
  * $n=pq$ с прости числа $p,q$
  * $ed \equiv 1 \pmod{φ(n)}$
* Енкрипция: $c = m^e \pmod n$
* Декрипция: $m = c^d \pmod n$

### Криптографски операции - синтез
* **Encryption**: симетрично или асиметрично шифроване (AES, RSA).
* **Hashing**: неизменяеми хеш-суми (SHA-256).
* **MAC/HMAC**: автентикация и проверка на данни.
* **Key exchange**: DH/ECDHE за безопасно споделяне на сесийни ключове.

* Asymmetric Encryption (Асиметрично криптиране)
   * Публичен ключ за криптиране, частен за декриптиране.

* **[Trusted Authorities](https://en.wikipedia.org/wiki/Certificate_authority)** (Доверени удостоверяващи органи)
   * **[CA](https://en.wikipedia.org/wiki/Certificate_authority)** валидират сертификати, **[CAA](https://en.wikipedia.org/wiki/DNS_Certification_Authority_Authorization)** ограничава издаването.

* **[Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy)** (PFS)
   * Защитеност, дори ако дългосрочното ключово материално се компроментира.
   * Използва се с ключов обмен DH/ECDHE.

### Key Exchange Protocols

#### [Diffie�Hellman](https://en.wikipedia.org/wiki/Diffie�Hellman_key_exchange) (DH)
* Протокол за сигурен обмен на ключове през несигурен канал.
* Позволява на две страни да създадат споделен таен ключ без да го изпращат директно.
* Базира се на математически трудността на дискретния логаритъм.

#### [ECDHE](https://en.wikipedia.org/wiki/Elliptic-curve_Diffie%E2%80%93Hellman) (Elliptic Curve [Diffie�Hellman](https://en.wikipedia.org/wiki/Diffie�Hellman_key_exchange) Exchange)
* Вариант на [Diffie�Hellman](https://en.wikipedia.org/wiki/Diffie�Hellman_key_exchange), използващ елиптични криви.
* По-ефективен и сигурен при по-малка дължина на ключовете.
* Предпочитан метод за постигане на Perfect Forward Secrecy в съвременните TLS конфигурации.

### [HSTS](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security) (HTTP Strict Transport Security)
* Механизъм за защита, който принуждава браузърите да комуникират със сървъра само през HTTPS.
* Предотвратява downgrade атаки и случайни HTTP заявки.
* Имплементира се чрез HTTP заглавка: `Strict-Transport-Security`.

---

## Атаки срещу TLS

### [SSLStrip](https://en.wikipedia.org/wiki/SSLStrip) атака
* **Описание**: Man-in-the-Middle атака, при която атакуващият прихваща връзката и превръща HTTPS заявки в HTTP.
* **Механизъм**:
  * Потребителят смята, че комуникира през HTTP (или HTTPS с атакуващия).
  * Атакуващият комуникира със сървъра през HTTPS.
  * Потребителят не е защитен с криптиране end-to-end.
* **Защита**: Използване на [HSTS](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security), което предотвратява downgrade към HTTP.

---

## Man-in-the-Middle ([MITM](https://en.wikipedia.org/wiki/Man-in-the-middle_attack))
Ситуация, в която атакуващият тайно прехваща и препредава комуникацията между две страни.
* **Защита**: Валидация на SSL/TLS сертификати и използване на доверени Certificate Authorities (CA).

---

## Пароли и хеширане

### Хеширане
* **Записваме паролите в хеширан вид**: `password_hash($password, PASSWORD_DEFAULT)` и `password_verify()`.
* **[Rainbow таблици](https://en.wikipedia.org/wiki/Rainbow_table)**: предварително генерирани хеш-стойности за широко пространство от пароли.
* **[reduce функция](https://en.wikipedia.org/wiki/Rainbow_table#Reducing_function)** (в rainbow): връща нова „заглушена" парола, използвана за верига и търсене.
* **дълбочина на rainbow таблица**: брой стъпки в верига (по-голям = по-широка покритие, но повече време/памет).
* **[salt](https://en.wikipedia.org/wiki/Salt_(cryptography))**: уникален случайно генериран низ, добавен към парола преди хеширане, блокира rainbow таблици.
* **[pepper](https://en.wikipedia.org/wiki/Pepper_(cryptography))**: тайна стойност, съхранявана отделно от базата, добавя още един слой.

### Криптоген атаки и защита
* **[Hash length extension attack](https://en.wikipedia.org/wiki/Length_extension_attack)**: атаките използват Merkle–Damgård функции (MD5/SHA1) за допълване с допълнителни байтове и валидни [MAC](https://en.wikipedia.org/wiki/Message_authentication_code).
* **[MAC](https://en.wikipedia.org/wiki/Message_authentication_code) (Message Authentication Code)**: $\mathrm{MAC}_K(m)$, комбинация от ключ и съобщение за защитена автентикация.
* **[HMAC](https://en.wikipedia.org/wiki/HMAC)**:  защита срещу length extension $$HMAC_K(m)=H((K\oplus opad)||H((K\oplus ipad)||m))$$
* **[Side-channel атака](https://en.wikipedia.org/wiki/Side-channel_attack)**: време/паметно бавене при [HMAC](https://en.wikipedia.org/wiki/HMAC) или хеши функции (например с `time-constant compare`).

### Бавни хеш функции / KDF
* **[PBKDF2](https://en.wikipedia.org/wiki/PBKDF2)**: $PBKDF2(P, S, c, dkLen)$ връща изведен ключ; брой итерации `c` прави атаката по-скъпа.
* **[Argon2](https://en.wikipedia.org/wiki/Argon2)**: модерна, изчислително и паметно натоварваща, нужна за защита на пароли.

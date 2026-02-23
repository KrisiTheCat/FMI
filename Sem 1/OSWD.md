### Session cookies
 - $_SESSION - асоциативен масив с информация за сесията, както и локални променливки в нея
 - id 
   - PHPSESSID - изпраща се от браузъра и съдържа id-то
 - **session hijacking** - stealing or compromising the unique session identifier 
 - **session fixation** - an attacker forces a known session ID onto a user's browser before they log in. Once the user authenticates, the attacker uses that pre-set ID to hijack the authenticated session, gaining full access to the user's account without needing their credentials.
  1. хакера ни дава айди с което да влезем на сървъра, пишем име и парола и те остават в сесията, така че хакера може да ползва сесията - ima reshenie
  2. хаакера ни дава негово ай ди и ние реално ползваме неговия акаунт - няма решение
   - решения:
     - IP fixation - не се препоръчва(макар да работи), защото ip-тата се сменят
     - browser fingerprint - не е ефикасно, ако са ни открили id-то, сигурно ни имат и това
     - session timeout - activity timer vs max session time
     - session_regenerate_id(true) - лошо за администраторите, сменя сесийното айди - хубаво е да го правим на ключови моменти - auth / password change / admin login etc (1)
      - true - преименува стария файл
      - false - kopira go i syzdawa now - legacy
     - да изкараме много индикатори на акаунта по страниците


### TLS Protocol
- криптиране - клиент и сървър пазят ключ за кодирането - AES256 
- кодиране - encoding - преобразуване по обратим алгоритъм - base64
- хеширане - преобразуване по необратим начин - SHA256


- асиметрично криптиране
- kpub, cpub - криптира
- kptr, cprt - декриптира

Примерни теми:
- генериране на рандъм числа

- 
###Man in the middle


# Практична робота № 1

**Дисципліна:** Основи побудови інформаційних систем та мереж

**Тема:** Спостереження за процесом звернення до вебресурсу. Побудова власної моделі рівнів взаємодії

| | |
|---|---|
| **Прізвище, ім'я** |Кладніцький Станіслав|
| **Група** |ЗПІ-21 |
| **Номер варіанта** |1 |
| **Домен варіанта** |example.com |
| **Середовище виконання** | Windows 11|
| **Версія curl** |curl 8.22.0 (Windows) libcurl/8.22.0 Schannel zlib/1.3.2 WinIDN WinLDAP|
| **Дата виконання** |25.11.2026|

---

## Частина A. Збір експериментальних даних

### A.1. Запит із діагностичним виводом

**Команда:**

```
 curl -v https://example.com
```

**Вивід:**

```
StatusCode        : 200
StatusDescription : OK
Content           : <!doctype html><html lang=en><head><meta charset=utf-8><link rel=icon
                    href=data:,><meta name=viewport
                    content="width=device-width,initial-scale=1"><title>Example
                    Domain</title><style>html{color-scheme...
RawContent        : HTTP/1.1 200 OK
                    Transfer-Encoding: chunked
                    Connection: keep-alive
                    allow: GET, HEAD
                    Age: 6
                    cf-cache-status: HIT
                    CF-RAY: a60002300c33f53c-WAW
                    alt-svc: h3=":443"; ma=86400
                    Accept-Ranges: bytes
                    C...
Forms             : {}
Headers           : {[Transfer-Encoding, chunked], [Connection, keep-alive], [allow, GET, HEAD],
                    [Age, 6]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 577
```

---

### A.2. Запит без захисту з'єднання

**Команда:**

```
curl -v http://neverssl.com
```

**Вивід:**

```
* Host neverssl.com:80 was resolved.
* IPv6: (none)
* IPv4: 34.223.124.45
*   Trying 34.223.124.45:80...
* connect to 34.223.124.45 port 80 from 0.0.0.0 port 51085 failed: Timed out
* Failed to connect to neverssl.com:80 after 21187 ms: Could not connect to server
* closing connection #0
curl: (28) Failed to connect to neverssl.com:80 after 21187 ms: Could not connect to server
```

---

### A.3. Запит до служби доменних імен

*Windows: `Resolve-DnsName example.com`*

**Команда (перше виконання):**

```
 Resolve-DnsName example.com 
```

**Вивід:**

```
Name                                           Type   TTL   Section    IPAddress
----                                           ----   ---   -------    ---------
example.com                                    AAAA   220   Answer     2606:4700:10::6814:179a
example.com                                    AAAA   220   Answer     2606:4700:10::ac42:93f3
example.com                                    A      174   Answer     104.20.23.154
example.com                                    A      174   Answer     172.66.147.243
```

**Команда (повторне виконання через 5–7 хвилин):**

```
Resolve-DnsName example.com
```

**Вивід:**

```
Name                                           Type   TTL   Section    IPAddress
----                                           ----   ---   -------    ---------
example.com                                    AAAA   248   Answer     2606:4700:10::6814:179a
example.com                                    AAAA   248   Answer     2606:4700:10::ac42:93f3
example.com                                    A      192   Answer     172.66.147.243
example.com                                    A      192   Answer     104.20.23.154
```

**Зафіксовані значення:**

| Параметр | Перше виконання | Повторне виконання |
|---|---|---|
| Час виконання (год:хв) |12:40 |12:45|
| IP-адреса |2606:4700:10::6814:179a|2606:4700:10::6814:179a|
| Значення TTL |220-174|248-192|

> Якщо друге значення TTL виявилося більшим за перше — це нормально: кеш резолвера встиг оновитися. Зафіксуйте як є.

---

### A.4. Контрольний ресурс

**Команда:**

```
curl -v https://google.com
```

**Вивід:**

```
StatusCode        : 200
StatusDescription : OK
Content           : <!doctype html><html itemscope="" itemtype="http://schema.org/WebPage"
                    lang="uk"><head><meta content="text/html; charset=UTF-8"
                    http-equiv="Content-Type"><meta content="/images/branding/googleg/1x/goo...
RawContent        : HTTP/1.1 200 OK
                    Content-Security-Policy-Report-Only: object-src 'none';base-uri
                    'self';script-src 'nonce-N9fd6U-xQuR-tHrlWMvntQ' 'strict-dynamic'
                    'report-sample' 'unsafe-eval' 'unsafe-inline' https: ...
Forms             : {f}
Headers           : {[Content-Security-Policy-Report-Only, object-src 'none';base-uri
                    'self';script-src 'nonce-N9fd6U-xQuR-tHrlWMvntQ' 'strict-dynamic'
                    'report-sample' 'unsafe-eval' 'unsafe-inline' https: http:;report-uri
                    https://csp.withgoogle.com/csp/gws/other-hp], [Accept-CH,
                    Sec-CH-Prefers-Color-Scheme], [X-XSS-Protection, 0], [X-Frame-Options,
                    SAMEORIGIN]...}
Images            : {@{innerHTML=; innerText=; outerHTML=<IMG style="BORDER-TOP-STYLE: none;
                    BORDER-LEFT-STYLE: none; BORDER-BOTTOM-STYLE: none; BORDER-RIGHT-STYLE: none;
                    DISPLAY: none" alt="" src="https://ssl.gstatic.com/gb/images/bar/al-icon.png"
                    width=24 height=24>; outerText=; tagName=IMG; style=BORDER-TOP-STYLE: none;
                    BORDER-LEFT-STYLE: none; BORDER-BOTTOM-STYLE: none; BORDER-RIGHT-STYLE: none;
                    DISPLAY: none; alt=; src=https://ssl.gstatic.com/gb/images/bar/al-icon.png;
                    width=24; height=24}, @{innerHTML=; innerText=; outerHTML=<IMG id=hplogo
                    style="PADDING-BOTTOM: 14px; PADDING-TOP: 28px; PADDING-LEFT: 0px;
                    PADDING-RIGHT: 0px" alt=Google src="/images/branding/google_wordmark/v1/1x/google
                    logo_color_white_background_272x92dp.png" width=272 height=92>; outerText=;
                    tagName=IMG; id=hplogo; style=PADDING-BOTTOM: 14px; PADDING-TOP: 28px;
                    PADDING-LEFT: 0px; PADDING-RIGHT: 0px; alt=Google; src=/images/branding/google_wo
                    rdmark/v1/1x/googlelogo_color_white_background_272x92dp.png; width=272;
                    height=92}, @{innerHTML=; innerText=; outerHTML=<IMG
                    id=tsuid_ZrwGa77eL8XywPAP9PqqcQ_1 style="CURSOR: pointer; RIGHT: 5px; POSITION:
                    absolute; Z-INDEX: 300; TOP: 4px" alt="" src="/textinputassistant/tia.png"
                    width=27 height=23 data-script-url="/textinputassistant/13/uk_tia.js">;
                    outerText=; tagName=IMG; id=tsuid_ZrwGa77eL8XywPAP9PqqcQ_1; style=CURSOR:
                    pointer; RIGHT: 5px; POSITION: absolute; Z-INDEX: 300; TOP: 4px; alt=;
                    src=/textinputassistant/tia.png; width=27; height=23;
                    data-script-url=/textinputassistant/13/uk_tia.js}}
InputFields       : {@{innerHTML=; innerText=; outerHTML=<INPUT type=hidden value=uk name=hl>;
                    outerText=; tagName=INPUT; type=hidden; value=uk; name=hl}, @{innerHTML=;
                    innerText=; outerHTML=<INPUT type=hidden value=hp name=source>; outerText=;
                    tagName=INPUT; type=hidden; value=hp; name=source}, @{innerHTML=; innerText=;
                    outerHTML=<INPUT type=hidden name=biw>; outerText=; tagName=INPUT; type=hidden;
                    name=biw}, @{innerHTML=; innerText=; outerHTML=<INPUT type=hidden name=bih>;
                    outerText=; tagName=INPUT; type=hidden; name=bih}...}
Links             : {@{innerHTML=Gmail; innerText=Gmail; outerHTML=<A aria-label="Gmail "
                    class=gb_da href="https://mail.google.com/mail/&amp;ogbl" target=_top
                    data-pid="23">Gmail</A>; outerText=Gmail; tagName=A; aria-label=Gmail ;
                    class=gb_da; href=https://mail.google.com/mail/&amp;ogbl; target=_top;
                    data-pid=23}, @{innerHTML=Зображення; innerText=Зображення; outerHTML=<A
                    aria-label="Пошук зображень " class=gb_da
                    href="https://www.google.com/imghp?hl=uk&amp;ogbl" target=_top
                    data-pid="2">Зображення</A>; outerText=Зображення; tagName=A; aria-label=Пошук
                    зображень ; class=gb_da; href=https://www.google.com/imghp?hl=uk&amp;ogbl;
                    target=_top; data-pid=2}, @{innerHTML=<SVG aria-hidden=true class=gb_H
                    viewbox="0 0 24 24" focusable="false"><PATH d="M6,8c1.1,0 2,-0.9 2,-2s-0.9,-2
                    -2,-2 -2,0.9 -2,2 0.9,2 2,2zM12,20c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2
                    0.9,2 2,2zM6,20c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2
                    2,2zM6,14c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2 2,2zM12,14c1.1,0
                    2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2 2,2zM16,6c0,1.1 0.9,2 2,2s2,-0.9
                    2,-2 -0.9,-2 -2,-2 -2,0.9 -2,2zM12,8c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2
                    0.9,2 2,2zM18,14c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2
                    2,2zM18,20c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2 2,2z"></PATH><IMG
                    style="BORDER-TOP-STYLE: none; BORDER-LEFT-STYLE: none; BORDER-BOTTOM-STYLE:
                    none; BORDER-RIGHT-STYLE: none; DISPLAY: none" alt=""
                    src="https://ssl.gstatic.com/gb/images/bar/al-icon.png" width=24
                    height=24></IMG></SVG>; innerText=; outerHTML=<A aria-expanded=false role=button
                    tabIndex=0 aria-label="Додатки Google" class=gb_C
                    href="https://www.google.com.ua/intl/uk/about/products"><SVG aria-hidden=true
                    class=gb_H viewbox="0 0 24 24" focusable="false"><PATH d="M6,8c1.1,0 2,-0.9
                    2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2 2,2zM12,20c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2
                    -2,0.9 -2,2 0.9,2 2,2zM6,20c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2
                    2,2zM6,14c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2 2,2zM12,14c1.1,0
                    2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2 2,2zM16,6c0,1.1 0.9,2 2,2s2,-0.9
                    2,-2 -0.9,-2 -2,-2 -2,0.9 -2,2zM12,8c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2
                    0.9,2 2,2zM18,14c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2
                    2,2zM18,20c1.1,0 2,-0.9 2,-2s-0.9,-2 -2,-2 -2,0.9 -2,2 0.9,2 2,2z"></PATH><IMG
                    style="BORDER-TOP-STYLE: none; BORDER-LEFT-STYLE: none; BORDER-BOTTOM-STYLE:
                    none; BORDER-RIGHT-STYLE: none; DISPLAY: none" alt=""
                    src="https://ssl.gstatic.com/gb/images/bar/al-icon.png" width=24
                    height=24></IMG></SVG></A>; outerText=; tagName=A; aria-expanded=false;
                    role=button; tabIndex=0; aria-label=Додатки Google; class=gb_C;
                    href=https://www.google.com.ua/intl/uk/about/products}, @{innerHTML=<SPAN
                    class=gb_fb>Увійти</SPAN>; innerText=Увійти; outerHTML=<A aria-label=Увійти
                    class="gb_bb gb_je gb_gb gb_3d" href="https://accounts.google.com/ServiceLogin?hl
                    =uk&amp;passive=true&amp;continue=https://www.google.com/&amp;ec=GAZAmgQ"
                    target=_top><SPAN class=gb_fb>Увійти</SPAN></A>; outerText=Увійти; tagName=A;
                    aria-label=Увійти; class=gb_bb gb_je gb_gb gb_3d; href=https://accounts.google.co
                    m/ServiceLogin?hl=uk&amp;passive=true&amp;continue=https://www.google.com/&amp;ec
                    =GAZAmgQ; target=_top}...}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 92914
```

---

### A.5. Ресурси з некоректною конфігурацією сертифіката

**Випадок 1**

```
curl -v https://expired.badssl.com
```

```
PS C:\WINDOWS\System32> curl.exe -v https://expired.badssl.com
* Host expired.badssl.com:443 was resolved.
* IPv6: (none)
* IPv4: 104.154.89.105
*   Trying 104.154.89.105:443...
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* schannel: next InitializeSecurityContext failed: SEC_E_CERT_EXPIRED (0x80090328) - The received certificate has expired.
* closing connection #0
curl: (35) schannel: next InitializeSecurityContext failed: SEC_E_CERT_EXPIRED (0x80090328) - The received certificate has expired.

```

**Випадок 2**

```
curl -v https://wrong.host.badssl.com
```

```
*   Trying 104.154.89.105:443...
* Host wrong.host.badssl.com:443 was resolved.
* IPv6: (none)
* IPv4: 104.154.89.105
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* schannel: SNI or certificate check failed: SEC_E_WRONG_PRINCIPAL (0x80090322) - The target principal name is incorrect.
* closing connection #0
curl: (60) schannel: SNI or certificate check failed: SEC_E_WRONG_PRINCIPAL (0x80090322) - The target principal name is incorrect.
More details here: [https://curl.se/docs/sslcerts.html](https://curl.se/docs/sslcerts.html)

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the webpage mentioned above.
```

**Випадок 3**

```
curl -v https://self-signed.badssl.com
```

```
* Host self-signed.badssl.com:443 was resolved.
* IPv6: (none)
* IPv4: 104.154.89.105
*   Trying 104.154.89.105:443...
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* schannel: SEC_E_UNTRUSTED_ROOT (0x80090325) - The certificate chain was issued by an authority that is not trusted.
* closing connection #0
curl: (60) schannel: SEC_E_UNTRUSTED_ROOT (0x80090325) - The certificate chain was issued by an authority that is not trusted.
More details here: https://curl.se/docs/sslcerts.html
curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the webpage mentioned above.
```

> Якщо використано альтернативний спосіб із параметром `--resolve` — зазначити це та навести фактичну команду.

---

## Частина B. Власна модель рівнів

**Кількість виділених груп:** 5

| № | Назва групи (власне формулювання) | Рядки виводу, віднесені до групи | Обґрунтування |
|---|---|---|---|
| 1 |Отримання прикладного ресурсу|<!doctype html><html lang=en>...<title>Example Domain</title>...</html> (A.1); <HTML><HEAD>...<TITLE>301 Moved</TITLE>...<A HREF="https://www.google.com/">here</A>.</BODY></HTML>|Це дані, які клієнт отримує від сервера|
| 2 |Формування та надсилання HTTP-запиту|> GET / HTTP/1.1
> Host: nginx.org
> User-Agent: curl/8.22.0
> Accept: */*
* Request completely sent off
< HTTP/1.1 200 OK
< HTTP/1.1 301 Moved Permanently
< Location: https://www.google.com/
< Server: nginx/1.31.8
< Content-Type: text/html; charset=utf-8
< Content-Length: 6503 (та інші рядки з < і >)|Відображають сформований клієнтом запит|
| 3 |Захищений канал TLS|* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* ALPN: server accepted http/1.1
* using HTTP/1.x
* schannel: remote party requests renegotiation
* schannel: SSL/TLS connection renegotiated
* schannel: next InitializeSecurityContext failed: SEC_E_CERT_EXPIRED...
* schannel: SNI or certificate check failed: SEC_E_WRONG_PRINCIPAL...
* schannel: SEC_E_UNTRUSTED_ROOT...|Описують узгодження шифрування та перевірку сертифіката|
| 4 |Транспортне з’єднання|* Host example.com:443 was resolved., Type   TTL   Section    IPAddress
AAAA 220 Answer 2606:4700:10::6814:179a
AAAA 220 Answer 2606:4700:10::ac42:93f3
A 174 Answer 104.20.23.154 A 174 Answer 172.66.147.243 |Показують встановлення з’єднання з вузлом призначення|
| 5 |Визначення мережевої адреси|* Trying 3.125.197.172:443...
* Established connection to nginx.org (3.125.197.172 port 443) from 10.11.1.136 port 53597
* connect to 34.223.124.45 port 80 from 0.0.0.0 port 51085 failed: Timed out
* Connection #0 to host nginx.org:443 left intact
* closing connection #0|Відображають перетворення доменного імені на IP-адресу|

*Групи впорядковано від найближчої до користувача (№ 1) до найближчої до апаратного забезпечення. Зайві рядки вилучити, за потреби — додати.*

**Рядки, які не вдалося віднести до жодної групи:*1*

| Рядок виводу | Причина утруднення |
|---|---|
|< Connection: keep-alive|Передається як заголовок HTTP-відповіді (група 2), але фактично керує станом мережевого з'єднання — не закривати його після відповіді|

---

## Контрольні питання

**1. Скільки рядків діагностичного виводу передує отриманню даних сторінки (завдання A.1)?**

> У моєму результаті перед отриманням даних сторінки відображається інформація про HTTP-відповідь. Статус запиту — 200 OK, що свідчить про успішне отримання сторінки. Точну кількість діагностичних рядків за наданим скороченим виводом визначити неможливо, оскільки повний вивід curl -v не наведено

**2. Які рядки наявні у виводі A.1 і відсутні у виводі A.2? Чим це зумовлено?**

> У A.1 отримано HTTP-відповідь 200 OK, заголовки сервера та HTML-вміст сторінки. У A.2 з'єднання не встановилося через тайм-аут, тому HTTP-відповідь і вміст сторінки відсутні. Це зумовлено тим, що запит A.1 виконувався через HTTPS, а A.2 — через HTTP, причому TCP-з'єднання з портом 80 не було встановлено

**3. Звідки у виводі з'явилося значення `443`, якщо його не було вказано в адресі?**

> Число 443 — це стандартний порт HTTPS. Якщо порт не вказано в адресі, програма curl автоматично використовує стандартний порт протоколу. Для HTTP стандартним є порт 80, а для HTTPS — 443

**4. Як змінилося значення TTL між двома запитами (A.3)? Що означає це число?**

> Під час першого запиту TTL становив 220 для записів AAAA та 174 для записів A. Під час повторного запиту значення становило відповідно 248 та 192. TTL (Time To Live) — це час у секундах, протягом якого DNS-запис може зберігатися в кеші резолвера. У моєму результаті повторно отримані значення TTL збільшилися.

**5. Чим відрізняються між собою три причини помилок із завдання A.5? Сформулювати кожну однією фразою.**

| Випадок | Причина недовіри |
|---|---|
| `expired` |Сертифікат має прострочений термін дії, тому система не може вважати його дійсним|
| `wrong.host` |Ім'я домену не відповідає імені, зазначеному в SSL/TLS-сертифікаті|
| `self-signed` |Сертифікат є самопідписаним або його ланцюжок веде до недовіреного центру сертифікації|

**6. Три рядки з власних виводів, про які не йшлося на лекції 1:**

| № | Рядок виводу | Джерело (номер завдання) |
|---|---|---|
| 1 |cf-cache-status: HIT|A.1 |
| 2 |SEC_E_CERT_EXPIRED (0x80090328)|A.5, випадок 1 |
| 3 |SEC_E_UNTRUSTED_ROOT (0x80090325)|A.5, випадок 3 |

## Висновки

**D.1. Що виявилося неочевидним або несподіваним**

> Під час виконання практичної роботи було перевірено доступність вебресурсів за допомогою curl, виконано DNS-запити командою Resolve-DnsName та досліджено реакцію клієнта на різні помилки SSL/TLS-сертифікатів.

Під час звернення до example.com та google.com було отримано статус 200 OK. Запит до neverssl.com не встановив з'єднання через тайм-аут. Порівняння DNS-виводу показало наявність записів A та AAAA і різні значення TTL у двох вимірюваннях. У трьох тестах SSL/TLS було зафіксовано помилки простроченого сертифіката, невідповідності імені хоста та недовіреного кореневого центру сертифікації.

Отже, результати демонструють важливість перевірки доступності мережевих ресурсів, коректності DNS-відповідей і справжності SSL/TLS-сертифікатів під час встановлення захищеного з'єднання.

**D.2. Чому саме така кількість груп у частині B**

*На якій підставі ухвалено рішення. Що змусило б його змінити.*

> Для побудови власної моделі я обрав 5 груп, оскільки така кількість дає змогу логічно розподілити рядки діагностичного виводу за їхнім призначенням і послідовністю взаємодії під час звернення до вебресурсу. Я враховував, яку інформацію показує кожен рядок: результат DNS-перетворення, встановлення з'єднання, захищений обмін даними, HTTP-відповідь сервера та отримання вмісту сторінки. Вважаю, що поділ на меншу кількість груп зробив би модель надто узагальненою, а більша кількість могла б призвести до зайвого подрібнення схожих процесів. Я змінив би кількість груп, якби під час аналізу інших ресурсів виявилися окремі етапи взаємодії, які суттєво відрізняються за функціями та не можуть бути логічно віднесені до вже створених груп.

**D.3. Питання, яке залишилося без відповіді**

> Чому під час повторного виконання DNS-запиту значення TTL може збільшуватися, хоча зазвичай очікується його зменшення? Хотілося б детальніше розібратися, як саме DNS-резолвер оновлює кеш записів і за яких умов отримує нове значення TTL. 

---

## Використання штучного інтелекту


**Факт використання:** використано 

**Установлений рівень для цієї роботи:** Р3 — ШІ як співвиконавець

**Фактичний рівень використання:** Р3

### Використані системи

| Система | Версія або модель | Період використання |
|---|---|---|
|ChatGPT |GPT-6 |24–25.11.2026 |

### Промпти

| № | Розділ роботи | Текст промпта |
|---|---|---|
| 1 |Висновки та використання ШІ| Допоможи розібратись |


### Підтвердження

Підтверджую, що всі наведені в цьому звіті виводи команд отримано мною особисто внаслідок фактичного виконання відповідних дій, а відомості цього розділу є повними та достовірними.

> Виводи `curl`, `dig` та інші артефакти не можуть бути згенеровані. Це стосується будь-якого рівня використання ШІ.

---

## Примітки виконавця

*(необов'язковий розділ: що не спрацювало, які команди довелося змінити, які виникли труднощі)*

> 

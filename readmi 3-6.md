# Домашнее задание к занятию "3.6. Компьютерные сети, лекция 1"

1. Работа c HTTP через телнет.
- Подключитесь утилитой телнет к сайту stackoverflow.com
`telnet stackoverflow.com 80`
- отправьте HTTP запрос
```bash
GET /questions HTTP/1.0
HOST: stackoverflow.com
[press enter]
[press enter]
```
- В ответе укажите полученный HTTP код, что он означает?
```commandline
GET /questions HTTP/1.0
HOST: stackoverflow.com

HTTP/1.1 301 Moved Permanently
cache-control: no-cache, no-store, must-revalidate
location: https://stackoverflow.com/questions
x-request-guid: 28681d87-3762-4669-b70a-1612be58b423
feature-policy: microphone 'none'; speaker 'none'
content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com
Accept-Ranges: bytes
Date: Sun, 17 Oct 2021 20:28:43 GMT
Via: 1.1 varnish
Connection: close
X-Served-By: cache-fra19138-FRA
X-Cache: MISS
X-Cache-Hits: 0
X-Timer: S1634502524.814156,VS0,VE93
Vary: Fastly-SSL
X-DNS-Prefetch-Control: off
Set-Cookie: prov=fa9ec3f6-a6f7-b2a3-c577-67942674a00c; domain=.stackoverflow.com; expires=Fri, 01-Jan-2055 00:00:00 GMT; path=/; HttpOnly

Connection closed by foreign host.


Код состояния HTTP 301 или Moved Permanently (с англ. — «Перемещено навсегда») — стандартный код ответа HTTP, получаемый в ответ от сервера в ситуации, когда запрошенный ресурс был на постоянной основе перемещён в новое месторасположение, и указывающий на то, что текущие ссылки, использующие данный URL, должны быть обновлены
```
2. Повторите задание 1 в браузере, используя консоль разработчика F12.
- откройте вкладку `Network`
- отправьте запрос http://stackoverflow.com
- найдите первый ответ HTTP сервера, откройте вкладку `Headers`
- укажите в ответе полученный HTTP код.
  ```commandline
        Request URL: https://stackoverflow.com/
        Request Method: GET
        Status Code: 200 
    ```

- проверьте время загрузки страницы, какой запрос обрабатывался дольше всего?
- приложите скриншот консоли браузера в ответ.


3. Какой IP адрес у вас в интернете?
   31.172.133.6
4. Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? Воспользуйтесь утилитой `whois`
```commandline
 whois 31.172.133.6
% This is the RIPE Database query service.
% The objects are in RPSL format.
%
% The RIPE Database is subject to Terms and Conditions.
% See http://www.ripe.net/db/support/db-terms-conditions.pdf

% Note: this output has been filtered.
%       To receive output for a database update, use the "-B" flag.

% Information related to '31.172.133.0 - 31.172.133.255'

% Abuse contact for '31.172.133.0 - 31.172.133.255' is 'admins@kmvtelecom.ru'

inetnum:        31.172.133.0 - 31.172.133.255
netname:        KMVtelecom-IP
country:        RU
admin-c:        JPNC-RIPE
tech-c:         JPNC-RIPE
status:         ASSIGNED PA
mnt-by:         POTOK-NCC
created:        2019-03-19T10:50:36Z
last-modified:  2019-03-19T10:50:36Z
source:         RIPE

role:           KMVtelecom NOC
address:        11/1 Ordzhonikidze st., Pyatigorsk, Russia
abuse-mailbox:  admins@kmvtelecom.ru
admin-c:        SL321-RIPE
tech-c:         NGAZ-RIPE
mnt-by:         POTOK-NCC
nic-hdl:        JPNC-RIPE
created:        2009-08-27T08:33:57Z
last-modified:  2015-11-04T14:49:30Z
source:         RIPE # Filtered

% Information related to '31.172.133.0/24AS44963'

route:          31.172.133.0/24
descr:          KMVtelecom_Stavropol_IP
origin:         AS44963
mnt-by:         POTOK-NCC
created:        2012-02-27T06:34:49Z
last-modified:  2019-08-21T06:00:39Z
source:         RIPE

% This query was served by the RIPE Database Query Service version 1.101 (WAGYU)
```

5. Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? Воспользуйтесь утилитой `traceroute`
6. Повторите задание 5 в утилите `mtr`. На каком участке наибольшая задержка - delay?
7. Какие DNS сервера отвечают за доменное имя dns.google? Какие A записи? воспользуйтесь утилитой `dig`
8. Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? воспользуйтесь утилитой `dig`
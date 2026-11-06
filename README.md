# AloraVPN Routing

Готовые `geosite.dat` и `geoip.dat` для маршрутизации Xray-совместимых
клиентов: российские и приватные сети — напрямую, выбранные зарубежные
сервисы — через VPN, рекламные домены — в блокировку.

Подходит для Xray-core, V2Ray, Happ, Incy и Remnawave.

## Файлы

| Файл | Назначение |
| --- | --- |
| [geosite.dat](./geosite.dat) | Доменные правила |
| [geoip.dat](./geoip.dat) | IPv4/IPv6-подсети |
| [aloravpn-smart-ru.json](./profiles/aloravpn-smart-ru.json) | Профиль маршрутизации Happ/Incy |
| [happ-routing-header.txt](./headers/happ-routing-header.txt) | Заголовок подписки для Happ |
| [incy-autorouting-header.txt](./headers/incy-autorouting-header.txt) | Заголовок подписки для Incy |
| [remnawave-response-headers.json](./headers/remnawave-response-headers.json) | Оба заголовка одним JSON-объектом |

## GitHub Raw-ссылки

```text
GeoIP:   https://raw.githubusercontent.com/TheLarrryLoveMom/AloraRouting/main/geoip.dat
Geosite: https://raw.githubusercontent.com/TheLarrryLoveMom/AloraRouting/main/geosite.dat
Profile: https://raw.githubusercontent.com/TheLarrryLoveMom/AloraRouting/main/profiles/aloravpn-smart-ru.json
```

## Happ и Incy в subscription response headers

Добавьте эти два ключа в JSON ответа подписки. Готовый объект можно взять из
[`headers/remnawave-response-headers.json`](./headers/remnawave-response-headers.json).

```json
{
  "routing": "значение из headers/happ-routing-header.txt без префикса routing: ",
  "autorouting": "значение из headers/incy-autorouting-header.txt без префикса autorouting: "
}
```

Для Happ используется ключ `routing` с deeplink `happ://routing/onadd/...`.
Для Incy — ключ `autorouting` с deeplink `incy://autorouting/onadd/...`.

В Remnawave вставьте содержимое JSON-файла в **Subscription → Settings →
Response Headers**. Если в объекте уже есть другие поля, добавьте только эти
два ключа. После изменения обновите подписку в приложении.

## Политика профиля AloraVPN Smart RU

- `direct`: private, RU, RU-CORE и CATEGORY-RU;
- `proxy`: Telegram, GitHub, Discord, WhatsApp, YouTube, AI, crypto и
  заблокированные ресурсы;
- `block`: CATEGORY-ADS.

Happ получает профиль из `routing`; Incy скачивает тот же профиль по URL из
`autorouting`. Оба клиента затем загружают актуальные DAT-файлы по ссылкам из
профиля.

## Проверка

```bash
curl -fIL https://raw.githubusercontent.com/TheLarrryLoveMom/AloraRouting/main/geoip.dat
curl -fIL https://raw.githubusercontent.com/TheLarrryLoveMom/AloraRouting/main/geosite.dat
curl -fL https://raw.githubusercontent.com/TheLarrryLoveMom/AloraRouting/main/profiles/aloravpn-smart-ru.json
```

## Документация

- [Xray routing](https://xtls.github.io/en/config/routing)
- [Happ routing](https://www.happ.su/main/dev-docs/routing)
- [Incy autorouting](https://docs.incy.cc/autorouting/)
- [Remnawave response rules](https://docs.rw/learn-en/routing-rules/)

## Обновление

При обновлении DAT-файлов меняйте `LastUpdated` в профиле и пересобирайте
`headers/happ-routing-header.txt`; Incy заберёт новую версию профиля при
следующей проверке обновлений.

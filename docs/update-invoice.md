# Оновлення данних інвойс

!!! warning

    Цей запит використовує [токен доступу](forming-requests.md#_3) та шифрування, що описанне у [формуванні запитів](forming-requests.md#_3).
    Крім того **оновити данні** може тільки користувач, що надіслав **emit** `invoice:join`, з UID заявки.

Для того щоб оновити данні в заявці вам потрібно, звернутися до `https://api.nltrade.in/method/RemoteInvoiceApi`, та передати йому JSON такої структури
``` json
{
   data: "ЗАШИФРОВАННИЙ_ВАШИМ_ПУБЛІЧНИМ_RSA_PKCS1_КЛЮЧЕМ_JSON",
   access_token: "ВАШ_ТОКЕН"
}
```
У полі data, ви повинні передати зашифрованний JSON такої структури
``` json
{
    "type": "update",
    "uid_link": invoiceUid,
    "log": "ОПЦІОНАЛЬНИЙ_ПАРАМЕТР_ВИКОРИСТОВУЄТЬСЯ_ДЛЯ_ДОДАВАННЯ_ЛОГУ_ЩОДО_ВИКОНАННИХ_ДІЙ_ЗІ_СТОРОНИ_КЛІЄНТА",
    ... Поля у базі данних, що треба оновити
}
```

## Значення що повертаються
В результаті перевірки повертається `JSON` поле `status`, типу `boolean`, його можна використовувати як поле для перевірки чи пройшов запит успішно.

## Загальний приклад використання 
У прикладі використовується, [функція](forming-requests.md#_5), що використовується у прикладі формуванні запитів.
``` js
let response = await apiRequest("https://api.nltrade.in/method/RemoteInvoiceApi", {"type": "update", "uid_link": invoiceUid, "log": "Заявка оплаченна клієнтом", ... поля що треба оновити}, token)
```

## Приклад оплати користувачем Invoic'у, 
У прикладі використовується, [функція](forming-requests.md#_5), що використовується у прикладі формуванні запитів.
``` js
let response = await apiRequest("https://api.nltrade.in/method/RemoteInvoiceApi", {"type": "update", "uid_link": invoiceUid, "status": "pay", "log": "Заявка оплаченна клієнтом"}, token)
```

## Приклад додавання фото користувачем Invoic'у, 
У прикладі використовується, [функція](forming-requests.md#_5), що використовується у прикладі формуванні запитів.
``` js
request = await apiRequest("https://api.nltrade.in/method/RemoteInvoiceApi", {"type": "update", "uid_link": invoiceUid, "status": "notApprove", "approve_photo": photoUrl, "log": "Фото підтвердження відправленно"}, token)
```

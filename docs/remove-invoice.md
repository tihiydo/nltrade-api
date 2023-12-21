# Видалення інвойсу

!!! warning

    Цей запит використовує [токен доступу](forming-requests.md#_3) та шифрування, що описанне у [формуванні запитів](forming-requests.md#_3).
Для того щоб видалити заявку вам потрібно, звернутися до `https://api.nltrade.in/method/RemoteInvoiceApi`, та передати йому JSON такої структури
``` json
{
   data: "ЗАШИФРОВАННИЙ_ВАШИМ_ПУБЛІЧНИМ_RSA_PKCS1_КЛЮЧЕМ_JSON",
   access_token: "ВАШ_ТОКЕН"
}
```
У полі data, ви повинні передати зашифрованний JSON такої структури
``` json
{
    "type": "remove",
    "uid_link": "UID_ІНВОЙСУ"
}
```

## Значення що повертаються
В результаті перевірки повертається `JSON` поле `status`, типу `boolean`, його можна використовувати як поле для перевірки чи пройшов запит успішно.

## Приклад використання
У прикладі використовується, [функція](forming-requests.md#_5), що використовується у прикладі формуванні запитів.
``` json
let response = await apiRequest("https://api.nltrade.in/method/RemoteInvoiceApi", {"type": "remove", "uid_link": uid}, token)
```

# Отримання інформації про Invoice

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
    "type": "get",
    "uid_link": "UID_ІНВОЙСУ"
}
```

## Значення що повертаються
В результаті перевірки повертається `JSON` поле `status`, типу `boolean`, його можна використовувати як поле для перевірки чи пройшов запит успішно.
Також в результаті запиту повертається массив з одним елементом всередині, що містить інформацію про Inoice.

## Приклад поверненного JSON
``` json
{
    "status": true,
    "response": {
        "data": {
            "invoice": [
                {
                    "id": "490",
                    "telegram_id": "24234234234",
                    "trader_id": "4",
                    "name_ex": "TestEx",
                    "send_value": "1",
                    "recive_value": "0.03",
                    "currency": "39.81",
                    "card_f_pay": "9999-9999-9999-9999",
                    "msg": null,
                    "approve_photo": null,
                    "technical": null,
                    "status": "notApprove",
                    "date": "2023-12-19 16:08:48",
                    "codeex": null,
                    "pay_info": null,
                    "trader_photo": null,
                    "pay_date": null,
                    "uid_link": "ngyt8761x4fvwo2jz0a5cr3ihubdle",
                    "ip": "37.72.44.30",
                    "payment_timeout": "960",
                    "accept_date": "2023-12-19 16:09:24",
                    "exchange_log": "[{\"date\":\"2023-12-19 16:08:48\",\"status\":\"Инвойс создан\"}",
                    "trader_change_price": null,
                    "exchange_type": "invoice",
                    "user_id": null
                }
            ]
        }
    }
}
```

## Типи статусів заявки
    'not approve exchange': 'Заявка ще не прийнята трейдером',
    'canceled': 'Скасовано',
    'rejected': 'Скасовано, через закінченння часу',
    "send card": 'Очікується оплата клієнтом',
    "wait card": 'Очікування картки від трейдера',
    'pay': 'Перевірка оплати трейдером',
    'success': 'Виконано успішно',
    'waitPhoto': 'Очікуванння фото підтвердження від клієнта',
    'notApprove': 'Адміністратор розглядає заявку'

## Приклад використання
В прикладі використовується [функція](forming-requests.md#_5), яка застосовується для створення запитів.
``` js
let response = await apiRequest("https://api.nltrade.in/method/RemoteInvoiceApi", {"type": "get", "uid_link": uid}, token)
```

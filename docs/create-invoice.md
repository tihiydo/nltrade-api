# Створення інвойсу

!!! warning

    Цей запит використовує [токен доступу](forming-requests.md#_3) та шифрування, що описанне у [формуванні запитів](forming-requests.md#_3).
Для того щоб створити заявку вам потрібно, звернутися до `https://api.nltrade.in/method/RemoteInvoiceApi`, та передати йому JSON такої структури
``` json
{
   data: "ЗАШИФРОВАННИЙ_ВАШИМ_ПУБЛІЧНИМ_RSA_PKCS1_КЛЮЧЕМ_JSON",
   access_token: "ВАШ_ТОКЕН"
}
```
У полі data, ви повинні передати зашифрованний JSON такої структури
``` json
{
    "type": "create",
    "title": "Назва Invoice",
    "price": "Сумма Invoice в UAH"
}
```

##Приклад використання 
let response = await apiRequest("https://api.nltrade.in/method/RemoteInvoiceApi", {"type": "create", "title": title, "price": price}, token)

# Формування запитів
Якщо ви хочете мати доступ до нашого API, вам потрібно мати свій токен, який можна створити на [WellPay](https://wellpay.me/).

При запитах ви будете отримувати відповідь приблизно такого виду:
``` json
{
   "status":true,
   "response":{
      "data":{
         ...
      }
   }
}
```
Ви можете використовувати status, як перевірку на те чи виконанний запит успішо чи ні. response не завжди буде у відповіді.

## Шифрування запитів
У кожного нашого користувача є пара RSA ключів. Для того, щоб звернутись до нашого API, ви повинні спочатку звернутися до метода `https://api.nltrade.in/helpers/getLatestPublicRemoteRsaKey.php`, що поверне вам публічний RSA ключ в зашифрованному форматі.
При використанні метода `https://api.nltrade.in/helpers/getLatestPublicRemoteRsaKey.php`, ви повинні передати у POST параметрі `access_token`, який ви отримали на [WellPay](https://wellpay.me/).

У результаті виконання запиту, ви отримаєте таку відповідь:
``` json
{
   "status":true,
   "response":{
      "data":{
         "result":"LS0tLS1PUlRWQSBFRk4gQ0..."
      }
   }
}
```
Після вам потрібно виконати розшифрування публічного RSA ключа.
Ключ дешифрується двома етапами. Шифром цезара, а після використанням base64 decode.
Вам потрібно змістити ваші символи у словарю на -13.

Приклад реалізації шфиру цезаря на Node.js/ES6
``` js
const caesarEncrypt = (str, amount) => 
{
    if (amount < 0) 
    {
        amount += 26;
    }

    let output = '';
    for (let i = 0; i < str.length; i++) 
    {
        let c = str.charAt(i);
        if (/[a-z]/i.test(c)) 
        {
            let code = str.charCodeAt(i);
            if (code >= 65 && code <= 90) 
            {
                c = String.fromCharCode(((code - 65 + amount) % 26) + 65);
            } 
            else if (code >= 97 && code <= 122) 
            {
                c = String.fromCharCode(((code - 97 + amount) % 26) + 97);
            }
        }
        output += c;
    }
    return output;
};
```

Тобто на Node.js/ES6 для дешифрування публічного RSA ключа ви могли б використати такий запис
``` js
const rsaKey = caesarEncrypt(atob(jsonResponse.response.data.result), -13)
```

Ваш ключ ви можете зберегти у себе локально на сервері/комп'ютері.

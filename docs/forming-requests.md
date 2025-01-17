# Формування запитів
## Вступна інформація
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
У кожного нашого користувача є пара RSA PKCS1 ключів. Для того, щоб звернутись до нашого API, ви повинні спочатку звернутися до метода `https://api.nltrade.in/helpers/getLatestPublicRemoteRsaKey.php`, що поверне вам публічний RSA ключ в зашифрованному форматі.
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
Вам потрібно змістити ваші символи у словарю на `-13`.

Приклад реалізації шифру цезаря на Node.js/ES6
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

##Побудова запиту 
При відпраці запиту на наше API, `Content-Type` запиту повиннен бути `application/json`,
У самому ж JSON повинно передаватись таке:
``` json
{
   data: "ЗАШИФРОВАННИЙ_ВАШИМ_ПУБЛІЧНИМ_RSA_PKCS1_КЛЮЧЕМ_JSON"
   access_token: ВАШ_ТОКЕН
}
```
В поле JSON data, ви повині розмістити JSON, що зашифрованний публічним RSA, PCS1 ключем.
Перед тим як зашифрувати ваші JSON данні, ви повинні додати ключ `verificationWord`, зі значенням `bandasosamba` який використовується на нашому сервері заради перевірки цілісності вашого шифрування.

Приклад не зашифрованного поля data (JSON поле, що містить JSON), що ви повинні зашифрувати, а потім передати на сервер:
``` json
{
   "type": "list",
   "verificationWord": "bandasosamba"
}
```

Приклад повного запиту, з зашифрованним поле `data`:
``` json
{
   "data": "GAglei+idcMYwHiMALs2hGr/Te6opknMMS0OhePiR..."
   "access_token": "vtz2mxq978k4clb51efrapsjdgio3uhy", 
}
```

##Приклад реалізації запиту
Приклад файлу `apiRequest.js`, який має в `export` функцію `apiRequest.js`, що може використовуватись у Node.js/ES6, для виконнаня запитів до нашого API:
``` js
import fetch from 'node-fetch';
import crypto from 'crypto';

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

const getLatestRsaPublicKey = async (access_token) =>
{
    const request = await fetch("https://api.nltrade.in/helpers/getLatestPublicRemoteRsaKey.php", { method: "POST", headers: {'Content-Type': 'application/x-www-form-urlencoded'}, body: `access_token=${access_token}`});
    const jsonResponse = await request.json()

    if(jsonResponse.response?.data?.result)
    {
        const rsaKey = caesarEncrypt(atob(jsonResponse.response.data.result), -13)
        return String(rsaKey);
    }
}

export const apiRequest = async (url, options, access_token) =>
{
    const publicKey = await getLatestRsaPublicKey(access_token);
    const verificationWord = "bandasosamba"

    const data = 
    {
        ...options,
        verificationWord,
    };

    const encrypted = crypto.publicEncrypt
    (
        {
            key: publicKey,
            padding: crypto.constants.RSA_PKCS1_PADDING
        }, 
        Buffer.from(JSON.stringify(formDataWithVerification), 'utf-8')
    ).toString('base64');

    const request = await fetch(url, 
    {
        method: 'POST',
        body: JSON.stringify({ data: encrypted, access_token }),
        headers: 
        {
            'Content-Type': 'application/json',
        }
    });

    const text = await request.text()
    let json = ''

    try 
    {
        json = JSON.parse(text)
    }
    catch(e)
    {
        throw new Error(text);
    }

    if (!request.ok) 
    {
        if (json.message) 
        {
            if (json.message === 'Your key is too old') 
            {
                await getLatestRsaPublicKey(access_token);
                return apiRequest(url, ...options, access_token);
            }
            else throw new Error(json.message);
        }
    }

    return json;
};
```

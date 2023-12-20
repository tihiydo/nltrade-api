# Формування запитів
Якщо ви хочете мати доступ до нашого API, вам потрібно мати свій токен, який можна створити на [WellPay](https://wellpay.me/).

У кожного нашого користувача є пара RSA ключів. Для того, щоб звернутись до нашого API, ви повинні спочатку звернутися до метода `https://api.nltrade.in/helpers/getLatestPublicRemoteRsaKey.php`, що поверне вам публічний RSA ключ в зашифрованному форматі.
При використанні метода `https://api.nltrade.in/helpers/getLatestPublicRemoteRsaKey.php`, ви повинні передати у POST параметрі `access_token`, який ви отримали на [WellPay](https://wellpay.me/).

У результаті виконання запиту, ви отримаєте таку відповідь
``` json
{
   "status":true,
   "response":{
      "data":{
         "result":"LS0tLS1PUlRWQSBFRk4gQ0hPWVZQIFhSTC0tLS0tDQpaVlZQUHRYUE50Uk5tUC96T1NVaTI2anZFWVhOS0RmaFRKcEpoMEJxdFpDZDJIM1VHdG5ob1NkcUc5R013b1FRDQp0NlVJaTk5anZjT0p3KzdwUS94UGFPeXZWdFVqbmFVMkUwQ3k1NGtIZ1ZhTENnV3pSZml6dU5rb1A5TVR0REpyDQpkeUIvUWlPSWhjaFh2SGtsM0YxeXVwMWdwcEZPViswT3liRTVGcG9PUmFZL3puK0VSUzJQU3FQVGhmQXhKZm9iDQpLWXd5M2RQZ2E4RjArQStORDZzaG5rK2s1Q3VJc3krTEMzYUFPTk5QUGxxcHFsZUpEcFFYRkVudEYrR25UaW1SDQpOREJiZFNkRXB1b1owUWI5eTdwdEpGaE8vNkRPU0lpWDhwWk4vdWFLM2ZsN2NjSU1zQ1lJdE9aZUlPYWt6TFJuDQpOeDQwVVVqaTV1SUc4OEc4V1pwTVlDY0cvZllOaFFuVEpoM0Yvcmg5TU02cVI4OWZUQjBhL2poYlZRMWxEZDluDQppVjlpRk9QcTcyVll3NVRTZzFXUWE2Wm1odmdJb0xDRThCeFVOdldLTGpGem5wcGZ0K0dLK0Zxd3FaWFZaYnhXDQozdGg2OXJqWlZZbEl2YU9NWitXSmRuY2RQRW9LMzBpbVNOREVhMFpwMlVIUlFhZGI1RnBOdllEL1pXeGRraDdaDQoybXZNbUtWOHdoMkkyZXFnaHNCaG9WOW81azZYekMyd2RBTFFzNlVrcng3NWVwNjVGV01lUlAvenJJZllRbElVDQpENTVvdTQ3ZnV3L1k3bjRjWHZaY2FobWN5dDhBOFRhQXI5d2VwalIzb1VuK3A5MlNyOTJrYVJRV3o4NENxSGVuDQpyTEpCZUgxV3JDMTR5VngvYXQwdU82MmxMZXgxbCtwVFVid3lGaFZVdks1bmZZZTV0djh1U3Q4UE5qUk5ORD09DQotLS0tLVJBUSBFRk4gQ0hPWVZQIFhSTC0tLS0t"
      }
   }
}
```

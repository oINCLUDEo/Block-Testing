# Тест-кейсы для REST API GET /products/{productId}/status?authToken=&recalculate=&owner =&region=

## TC-01: Проверка успешных ответов с productStatus = 1

**Severity:** Critical

**Предусловия:**
- Для каждого набора параметров создаётся продукт с productStatus = 1
- Валидный authToken

**Шаги:**
1. Создать продукт с productStatus=1 и указанными параметрами
2. Отправить GET /products/{productId}/status с валидным authToken и параметрами recalculate, owner, region
3. Проверить, что HTTP 200 и в JSON ответе `"productStatus": 1`

**Ожидаемый результат:**
- HTTP 200 OK
- Тело: `{"productStatus": 1}`

| №   | recalculate | owner        | region       |
| --- | ----------- | ------------ | ------------ |
| 1   | true        | Пользователь | null         |
| 2   | null        | Пользователь | Сибирь       |
| 3   | null        | Создатель    | null         |
| 4   | false       | null         | Сибирь       |
| 5   | true        | Создатель    | Сибирь       |
| 6   | true        | отсутствует  | Поволжье     |
| 7   | null        | Создатель    | Поволжье     |
| 8   | null        | null         | null         |
| 9   | null        | Создатель    | Северо-Запад |
| 10  | false       | Создатель    | null         |
| 11  | true        | Пользователь | Северо-Запад |
| 12  | false       | Пользователь | Поволжье     |
| 13  | false       | null         | Северо-Запад |

## TC-02: Проверка успешных ответов с productStatus = 0

**Severity:** Critical

**Предусловия:**
- Для каждого набора параметров создаётся продукт с productStatus = 0
- Валидный authToken

**Шаги:**
1. Создать продукт с productStatus=0 и указанными параметрами
2. Отправить GET /products/{productId}/status с валидным authToken и параметрами recalculate, owner, region
3. Проверить, что HTTP 200 и в JSON ответе `"productStatus": 0`

**Ожидаемый результат:**
- HTTP 200 OK
- Тело: `{"productStatus": 0}`

| №   | recalculate | owner        | region       |
| --- | ----------- | ------------ | ------------ |
| 1   | true        | Пользователь | null         |
| 2   | null        | Пользователь | Сибирь       |
| 3   | null        | Создатель    | null         |
| 4   | false       | null         | Сибирь       |
| 5   | true        | Создатель    | Сибирь       |
| 6   | true        | отсутствует  | Поволжье     |
| 7   | null        | Создатель    | Поволжье     |
| 8   | null        | null         | null         |
| 9   | null        | Создатель    | Северо-Запад |
| 10  | false       | Создатель    | null         |
| 11  | true        | Пользователь | Северо-Запад |
| 12  | false       | Пользователь | Поволжье     |
| 13  | false       | null         | Северо-Запад |

## TC-03: Проверка авторизации: ошибки при некорректном или отсутствующем authToken

**Severity:** Critical

**Предусловия:**
- productId существует
- Параметры recalculate, owner, region любые

**Шаги:**
1. Отправить GET /products/{productId}/status с указанным authToken

**Ожидаемый результат:**
- HTTP 401 Unauthorized

| №   | authToken         | Описание                            |
| --- | ----------------- | ----------------------------------- |
| 1   |                   | Нет параметра authToken             |
| 2   | abc123            | Токен меньше 16 символов            |
| 3   | 12345678901234567 | Токен больше 16 символов            |
| 4   | 123456789012345X  | Токен валидной длины, но невалидный |

## TC-04: Проверка ошибки "продукт не найден"

**Severity:** Critical

**Предусловия:**
- productId отсутствует в системе
- валидный authToken

**Шаги:**
1. Отправить GET /products/{несуществующийId}/status

**Ожидаемый результат:**
- HTTP 404 Not Found

## TC-05: Проверка обработки внутренней ошибки сервера (500)

**Severity:** Blocker

**Предусловия:**
- Искусственно эмулировать сбой на сервер
- Валидный authToken
- Существующий productId

**Шаги:**
1. Отправить GET /products/{productId}/status

**Ожидаемый результат:**
- HTTP 500 Internal Server Error
- Тело: `{"errorMessage": "..."}`

## TC-06: Проверка обработки некорректных значений параметров recalculate, owner и region

**Severity:** Critical

**Предусловия:**
- productId существует
- Валидный authToken

**Шаги:**
1. Отправить GET /products/{productId}/status с некорректными значениями одного из параметров: recalculate, owner, region

**Ожидаемый результат:**
- HTTP 500 Internal Server Error
- Тело: `{"errorMessage": "..."}`

| №   | Параметр    | Значение            |
| --- | ----------- | ------------------- |
| 1   | recalculate | невалидное значение |
| 2   | owner       | невалидное значение |
| 3   | region      | невалидное значение |

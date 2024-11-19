# AWS GPT Classifier

Решение на базе AWS API Gateway и AWS Lambda для обработки и классификации наименования номенклатуры по классам и атрибутам c помощью ChatGPT с использованием Redis в качестве хранилища данных (классификатор)

## Описание проекта

Этот проект реализует базовое API с использованием AWS Lambda, включающее три метода:

- **`GET /classify`**: Классифицирует переданное наименование номенклатуры на основе классификатора, хранящегося в Redis
- **`POST /pickAtt`**: Подбирает атрибуты для переданного наименования номенклатуры на основе классификатора, хранящегося в Redis
- **`POST /updateClassifier`**: Обновляет классификатор, сохраняя данные в Redis

## Структура проекта

Проект разделён на три отдельных Lambda-функции, каждая из которых обрабатывает свой эндпоинт:

- **`/src/classify.py`**: Обрабатывает запросы `GET /classify`.
- **`/src/pick_attributes.py`**: Обрабатывает запросы `POST /pickAtt`.
- **`/src/update_classifier.py`**: Обрабатывает запросы `POST /updateClassifier`.

## Структура данных в Redis

Для хранения классификатора и атрибутов используется база данных Redis (развернут в AWS рядом с Lambda-функциями). В этом разделе описана структура данных, которая используется для хранения информации в Redis.

### Ключи и значения

В Redis данные организованы в формате ключ-значение. Каждая запись классификатора хранится следующим образом:

1. **Ключи категорий**:
   - Ключи категорий хранятся в формате:
     - `000000000000001`
     - `000000000000011`
     - `000000000000012`
     - `000000000000121`
     - и тд (таким образом реализована иерархия)
   - Значение для каждой категории представляет собой JSON-объект с информацией о соответствующих наименованиях и атрибутах

2. **Примеры хранения категорий и атрибутов**:

#### Пример данных в Redis:

```plaintext
000000057030105
```
```plaintext
{
   "code":"000000057030105",
   "name":"Хроматографы, анализаторы состава веществ",
   "attributes":[
      {
         "attribute_name":"Вид продукции",
         "attribute_type":"Текст",
         "attribute_example":"Хроматограф"
      },
      {
         "attribute_name":"Тип",
         "attribute_type":"Текст",
         "attribute_example":"газовый"
      },
      {
         "attribute_name":"Назначение",
         "attribute_type":"Текст",
         "attribute_example":"для определения содержания кислорода и кислородсодержащих соединений"
      },
      {
         "attribute_name":"Диапазон рабочих температур, C",
         "attribute_type":"Текст",
         "attribute_example":"5C...400C"
      },
      {
         "attribute_name":"Диапазон входного давления, МПа",
         "attribute_type":"Текст",
         "attribute_example":"0,36-0,44"
      },
      {
         "attribute_name":"Комплектация",
         "attribute_type":"Текст",
         "attribute_example":"с переключающимися колонками и контроллером"
      },
      {
         "attribute_name":"Опросный лист",
         "attribute_type":"Текст",
         "attribute_example":"2020-013-Р-1-0-ТХ.ОЛ46"
      }
   ]
}

# LabSber

```Python
from datetime import datetime
import json

# Загрузка данных из файла JSON
with open('operations.json', 'r', encoding='utf-8') as file:
    data = json.load(file)

# Фильтрация операций: оставляем только те, у которых дата не пуста и state равен "EXECUTED" или отсутствует
filtered_operations = [operation for operation in data if operation.get('date') and (not operation.get('state') or operation.get('state') == 'EXECUTED')]

# Сортировка операций по дате в обратном порядке (последние операции сверху)
sorted_data = sorted(filtered_operations, key=lambda x: datetime.strptime(x['date'], "%Y-%m-%dT%H:%M:%S.%f"), reverse=True)

# Вывод последних 5 выполненных операций
for operation in sorted_data[:5]:
    formatted_date = datetime.strptime(operation['date'], "%Y-%m-%dT%H:%M:%S.%f").strftime("%d.%m.%Y")
    from_account = operation.get('from', 'Отсутствует')
    to_account = operation['to']
    amount = operation['operationAmount']['amount']
    currency = operation['operationAmount']['currency']['name']

    # Маскирование номера карты и счета
    from_account = from_account[:6] + ' XX** **** ' + from_account[-4:]
    to_account = '**' + to_account[-4:]

    print(f"{formatted_date} {operation['description']}")
    print(f"{from_account} -> {to_account}")
    print(f"{amount} {currency}")
    print()
```

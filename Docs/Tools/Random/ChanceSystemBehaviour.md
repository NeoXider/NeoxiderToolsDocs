# Компонент Chance System Behaviour

## 1. Введение

`ChanceSystemBehaviour` — это компонент `MonoBehaviour`, который позволяет легко интегрировать систему взвешенных вероятностей `ChanceManager` прямо в вашу сцену Unity. Он предоставляет удобный интерфейс для настройки шансов в инспекторе и вызывает `UnityEvent` при генерации случайного ID.

Это идеальное решение для создания динамических событий, таких как выпадение лута, случайные встречи или выбор следующего действия AI, где все настройки можно выполнить без написания кода.

---

## 2. Описание класса

### ChanceSystemBehaviour
- **Пространство имен**: `Neo.Tools`
- **Путь к файлу**: `Assets/Neoxider/Scripts/Tools/Random/ChanceSystemBehaviour.cs`

**Описание**
Этот компонент является оберткой для `ChanceManager`, позволяя использовать его функциональность на `GameObject` в сцене. Он содержит экземпляр `ChanceManager` и предоставляет публичные методы и события для взаимодействия с ним.

**Ключевые поля**
- `_chanceManager` (`ChanceManager`): Встроенный экземпляр `ChanceManager`. Все настройки вероятностей (список `chances`, `preserveFirstChance`) производятся через это поле в инспекторе.
- `_chanceData` (`ChanceData`): (Опционально) Ссылка на ScriptableObject `ChanceData`. Если задан, компонент загрузит свою конфигурацию шансов из этого ассета при старте.

**Публичные методы (Public Methods)**
- `GenerateId()`: **Основной метод для использования.** Генерирует случайный ID на основе настроенных вероятностей и вызывает событие `OnIdGenerated`.
- `GetId()`: Возвращает случайный ID без вызова события.
- `AddChance(float probability)`: Добавляет новую вероятность.
- `RemoveChance(int index)`: Удаляет вероятность по индексу.
- `GetChance(int index)`: Возвращает значение вероятности по индексу.
- `SetChance(int index, float value)`: Устанавливает значение вероятности по индексу.
- `LoadFromChanceData(ChanceData data)`: Загружает конфигурацию шансов из указанного `ChanceData` ScriptableObject.
- `ClearChances()`: Очищает все настроенные вероятности.

**Unity Events**
- `OnIdGenerated` (`UnityEvent<int>`): Вызывается каждый раз, когда вызывается метод `GenerateId()`. Передает сгенерированный ID.

---

## 3. Как использовать

1.  Добавьте компонент `ChanceSystemBehaviour` на любой `GameObject` в вашей сцене.
2.  **Настройте вероятности**:
    -   **В инспекторе**: Разверните поле `_chanceManager` и настройте список `chances` и `preserveFirstChance`.
    -   **Через ScriptableObject**: Создайте ассет `ChanceData` (Right click > Create > Neoxider > Random > ChanceData), настройте его, а затем перетащите в поле `_chanceData` компонента `ChanceSystemBehaviour`.
3.  **Запустите генерацию**: Вызовите метод `GenerateId()` из другого скрипта или привяжите его к событию (например, к кнопке UI).
4.  **Реагируйте на результат**: Подпишитесь на событие `OnIdGenerated` в инспекторе, чтобы получить сгенерированный ID и выполнить соответствующую логику.

    ```csharp
    // Пример: скрипт, который реагирует на сгенерированный ID
    public class LootReceiver : MonoBehaviour
    {
        public void OnLootGenerated(int lootId)
        {
            switch (lootId)
            {
                case 0: Debug.Log("Получен обычный лут!"); break;
                case 1: Debug.Log("Получен редкий лут!"); break;
                // ... и так далее
            }
        }
    }
    ```

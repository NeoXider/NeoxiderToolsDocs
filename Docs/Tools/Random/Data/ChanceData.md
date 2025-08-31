# ScriptableObject Chance Data

## 1. Введение

`ChanceData` — это `ScriptableObject`, который позволяет вам создавать и хранить конфигурации вероятностей (`ChanceManager`) в виде отдельных ассетов в вашем проекте Unity. Это очень удобно для:
- **Балансировки игры**: Вы можете легко настраивать шансы выпадения предметов, появления врагов или других случайных событий, не меняя код.
- **Переиспользования**: Одна и та же таблица шансов может быть использована в разных сценах или разными компонентами.
- **Удобства для геймдизайнеров**: Геймдизайнеры могут настраивать вероятности напрямую в инспекторе, не затрагивая скрипты.

---

## 2. Описание класса

### ChanceData
- **Пространство имен**: `Neo.Tools`
- **Путь к файлу**: `Assets/Neoxider/Scripts/Tools/Random/Data/ChanceData.cs`
- **Тип**: `ScriptableObject`

**Описание**
Этот ассет содержит экземпляр `ChanceManager`, который управляет списком вероятностей. Вы можете настроить этот список прямо в инспекторе ассета. При вызове метода `GenerateId()` на этом ассете, он вернет случайный ID на основе настроенных вероятностей и вызовет событие `OnIdGenerated`.

**Ключевые поля**
- `_chanceManager` (`ChanceManager`): Встроенный экземпляр `ChanceManager`. Все настройки вероятностей (список `chances`, `preserveFirstChance`) производятся через это поле в инспекторе ассета.

**Публичные методы (Public Methods)**
- `GenerateId()`: **Основной метод для использования.** Генерирует случайный ID на основе настроенных вероятностей и вызывает событие `OnIdGenerated`.
- `AddChance(float probability)`: Добавляет новую вероятность в список `_chanceManager`.
- `RemoveChance(int index)`: Удаляет вероятность по индексу.
- `GetChance(int index)`: Возвращает значение вероятности по индексу.
- `SetChance(int index, float value)`: Устанавливает значение вероятности по индексу.
- `ClearChances()`: Очищает все настроенные вероятности.

**Unity Events**
- `OnIdGenerated` (`UnityEvent<int>`): Вызывается каждый раз, когда вызывается метод `GenerateId()` на этом ассете. Передает сгенерированный ID.

---

## 3. Как использовать

1.  **Создание ассета**: В окне Project (Assets) кликните правой кнопкой мыши -> Create -> Neoxider -> Random -> ChanceData. Дайте новому ассету осмысленное имя (например, `LootDropChances`).
2.  **Настройка вероятностей**: Выберите созданный ассет в окне Project. В инспекторе настройте список `chances` внутри `_chanceManager`, как вы бы это делали для `ChanceSystemBehaviour`.
3.  **Использование в скриптах**: Вы можете ссылаться на этот ассет из любого `MonoBehaviour` или другого ScriptableObject.

    ```csharp
    using Neo.Tools;
    using UnityEngine;

    public class MyGameLogic : MonoBehaviour
    {
        [SerializeField] private ChanceData _myLootChances;

        public void DropRandomLoot()
        {
            int droppedItemId = _myLootChances.GenerateId();
            Debug.Log($"Выпал предмет с ID: {droppedItemId}");
            // Дальнейшая логика обработки выпавшего предмета
        }
    }
    ```

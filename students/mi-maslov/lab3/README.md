# Лабораторная работа №3. Наивный байесовский классификатор

## Описание работы

В рамках данной лабораторной работы был реализован **наивный байесовский классификатор** с гауссовским распределением для задачи многоклассовой классификации и проведено сравнение с эталонной реализацией из библиотеки scikit-learn.

## Описание датасета

Для экспериментов использовался классический датасет **Iris** из библиотеки scikit-learn:
- **Количество объектов**: 150
- **Количество признаков**: 4 (длина и ширина чашелистика и лепестка)
- **Количество классов**: 3 (setosa, versicolor, virginica)
- **Разделение**: 80% для обучения (120 объектов), 20% для тестирования (30 объектов)

## Реализованный алгоритм

### Наивный байесовский классификатор (NaiveBayesClassifier)

Алгоритм основан на теореме Байеса с предположением о независимости признаков. 

**Ключевые особенности реализации:**

1. **Обучение (fit)**:
   - Вычисление априорных вероятностей классов: `P(y)`
   - Оценка параметров гауссовского распределения для каждого признака в каждом классе
   - Сохранение средних значений и стандартных отклонений

2. **Предсказание (predict)**:
   - Вычисление плотности вероятности через гауссовскую функцию
   - Применение теоремы Байеса для вычисления апостериорных вероятностей
   - Выбор класса с максимальной вероятностью

3. **Функция плотности вероятности**:
   ```
   P(x|μ,σ) = (1/√(2πσ²)) * exp(-(x-μ)²/(2σ²))
   ```

4. **Обработка особых случаев**:
   - Защита от деления на ноль при σ = 0 (замена на малое значение 1e-9)

**Математическая основа:**
- Теорема Байеса: `P(y|X) = P(X|y) * P(y) / P(X)`
- Предположение независимости: `P(X|y) = ∏P(xi|y)`
- Классификация: `y* = argmax P(y|X)`

## Результаты экспериментов

### Основное сравнение

**Результаты на тестовой выборке:**

| Метрика | Собственная реализация | Scikit-learn |
|---------|----------------------|--------------|
| **Точность** | 1.0000 (100%) | 1.0000 (100%) |
| **Время обучения** | 0.001941 сек | 0.002384 сек |
| **Время предсказания** | 0.003752 сек | 0.001628 сек |

### Кросс-валидация (5-fold)

**Результаты собственной реализации:**
- **Среднее значение точности**: ~0.96 (96%)
- **Стандартное отклонение**: ~0.05
- **Точности по фолдам**: Варьируются от ~0.90 до 1.00

Кросс-валидация показывает стабильную работу алгоритма с высокой точностью на различных разбиениях данных.

### Анализ результатов

**Качество модели:**
- Обе реализации показали идеальную точность (100%) на тестовой выборке
- Кросс-валидация подтверждает высокое качество с средней точностью ~96%
- Алгоритм хорошо справляется с данной задачей классификации

**Производительность:**
- Собственная реализация обучается быстрее библиотечной (0.0019 vs 0.0024 сек)
- Scikit-learn быстрее делает предсказания (0.0016 vs 0.0038 сек)
- Общее время работы сопоставимо для обеих реализаций

## Описание наивного байесовского классификатора

Наивный байесовский классификатор — это вероятностный алгоритм машинного обучения, основанный на теореме Байеса с "наивным" предположением о независимости признаков.

**Основные принципы:**

1. **Теорема Байеса**: Связывает априорную и апостериорную вероятности
2. **Предположение независимости**: Все признаки независимы друг от друга 

## Выводы

1. **Корректность реализации**: Собственная реализация наивного байесовского классификатора работает корректно и показывает результаты, идентичные библиотечной версии на тестовых данных.

2. **Эффективность алгоритма**: Наивный Байес отлично справляется с задачей классификации ирисов, достигая 100% точности на тестовой выборке, что подтверждает его эффективность для данного типа задач.

3. **Производительность**: Обе реализации показывают сопоставимую производительность, при этом собственная реализация даже превосходит scikit-learn по времени обучения.

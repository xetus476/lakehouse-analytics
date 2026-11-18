# Кейс 2. Предсказание продолжительности матча (Match Duration Prediction)

Разработать регрессионную модель для предсказания продолжительности матча на основе событий ранней стадии игры.

Предсказывать продолжительность матча необходимо, используя исключительно информацию, доступную в первые 15 минут игры.

Сравнить базовую модель и регрессионную модель на различных временных интервалах.

## Гипотезы

1. Если обучить модель на данных ранней стадии игры (0–15 минут), экономическое преимущество (разница в золоте и опыте между командами) войдёт в тройку наиболее важных признаков. Это объясняется эффектом снежного кома (Snowball Effect): раннее преимущество одной команды позволяет ей быстрее завершить матч.
2. Если обучить модель на объединённом наборе данных, включающем матчи в режиме Turbo, точность предсказания (MAE) для стандартных матчей снизится. Это связано с особенностями механик Turbo — ускоренным получением ресурсов и изменённым игровым таймингом, которые вносят дополнительный шум и искажают зависимость между событиями первых 15 минут и общей продолжительностью матча.
3. При сравнении базовой модели с деревом решений дерево решений будет показывать лучшие результаты по мере увеличения объёма данных, поскольку игровое преимущество и темп матча накапливаются со временем.

## Источники данных

- `dota_duration_15min_matches`
- `dota_duration_15min_match_players`
- `dota_duration_15min_kill_events`
- `dota_duration_15min_gold_events`
- `dota_duration_15min_tower_death_events`
- `dota_duration_15min_player_team_map`

## Обработка данных

1. Исключаются матчи продолжительностью менее 15 минут.
2. Исключаются матчи, в которых количество игроков хотя бы в одной команде не равно 5.
3. Удаляются дубликаты, возникающие в результате операций JOIN между таблицами.
4. Исключаются игроки-боты, определяемые по их `id`.
5. Игроки каждого матча распределяются по командам `dire` и `radiant`.

## Методология

После выполнения всех преобразований данных набор разделяется на обучающую и тестовую выборки.

Затем обучающая выборка используется для построения двух моделей:
- **Регрессионная модель** — базовая модель для предсказания продолжительности матча.
- **Random Forest** — ансамблевая модель на основе деревьев решений.

После обучения выполняется оценка качества предсказаний обеих моделей и сравнение полученных результатов.

## Результат обработки

`dota_duration_15min_match_duration_predictions` — итоговая таблица, содержащая основные признаки, необходимые для анализа, а также результаты предсказаний обеих моделей.

Данная таблица была установлена в Linage, после чего все необходимые значения были переданы в Apache Superset для визуализации.

## Визуализация

### Чем больше преимущество команды к 15-й минуте, тем проще предсказать исход матча

<img width="1094" height="535" alt="изображение" src="https://github.com/user-attachments/assets/455e5d2d-24fe-42c3-aabd-048672e24cdc" />

### Сходимость результатов двух моделей

<img width="1058" height="501" alt="изображение" src="https://github.com/user-attachments/assets/d257eaa1-d411-4ea4-8428-c145aa02fa45" />

## Проверка гипотез

### Проверка гипотез — Кейс 19

### 1. Экономическое преимущество (разница в золоте и опыте) входит в тройку наиболее важных факторов, влияющих на продолжительность матча.

Для оценки важности признаков использовалась модель дерева решений.

Выбросы были удалены с помощью правила трёх сигм (3-Sigma Rule).

Размер выборки: **13 048 матчей**.

### 2. Режим Turbo как фактор, влияющий на точность предсказания.

Особенности режима Turbo могут влиять на взаимосвязь между событиями первых 15 минут и общей продолжительностью матча.

Ускоренное получение ресурсов и изменённый темп игры способны ухудшать качество модели, обученной на объединённых данных, при предсказании продолжительности стандартных матчей.

### 3. Модель дерева решений превосходит базовую модель, а разница в качестве увеличивается со временем.

Для сравнения базовой модели (медиана) и модели дерева решений были рассчитаны метрики MAE и RMSE на временных интервалах 5, 10 и 15 минут.

## Выводы

### Предсказание продолжительности матча

Для предсказания продолжительности матчей была обучена модель **Random Forest** на выборке из **13 048 матчей**.

Основные результаты:

- **MAE (Mean Absolute Error)** — 4,28 минуты.
- **RMSE (Root Mean Squared Error)** — 6,23 минуты.
- Экономические показатели также оказались значимыми признаками для предсказания продолжительности матча.
- Для матчей продолжительностью более 45 минут ошибка предсказания значительно выше и составляет приблизительно **16,33 минуты**.

Таким образом, модель Random Forest позволяет предсказывать продолжительность матча на основе событий первых 15 минут. При этом точность предсказания зависит от продолжительности матча: для особенно долгих игр ошибка существенно возрастает.

# Case 2
Build a regression model for match duration based on early-game events.
Predict the duration using only information available within the first 15 minutes. Compare the baseline and the regression model across different time segments.

## Hypotheses
1. If a model is trained on early-game data (0–15 minutes), the economic imbalance (the difference in gold/experience) will rank among the top three most important features, as an early "snowball" effect allows a team to end the match sooner.
2. If a model is trained on a combined dataset (including Turbo mode matches), the prediction accuracy (MAE) for standard matches will decrease; this is because Turbo mechanics—such as accelerated farming and different timings—introduce noise, distorting the relationship between events in the first 15 minutes and the total match duration.
3. When comparing a baseline model to a decision tree, the decision tree will yield better results as the amount of data increases, since the game's momentum accumulates over time.

# Data source
dota_duration_15min_matches
dota_duration_15min_match_players
dota_duration_15min_kill_events
dota_duration_15min_gold_events
dota_duration_15min_tower_death_events
dota_duration_15min_player_team_map

# Data Processing
1. Matches lasting less than 15 minutes are excluded.
2. Matches in which the number of players on at least one team is not equal to 5 are excluded.
3. Duplicates resulting from table JOIN operations are removed.
4. Bot players, identified by their `id`, are excluded.
5. Players in a match are categorized into `dire` and `radiant`.

# Methodology
After all data transformations,We split the data into training and test sets. We then feed the training data into two models: a regression model and a Random Forest. Finally, we compare the results from both models.

# Output
dota_duration_15min_match_duration_predictions - which contains all the essential information and the results of two models, we installed this table on Linage and transfer all the values ​​to the superset

# Visualization
## The greater the advantage by the 15-minute mark, the easier it is to predict the outcome of the match.
<img width="1094" height="535" alt="изображение" src="https://github.com/user-attachments/assets/455e5d2d-24fe-42c3-aabd-048672e24cdc" />

## convergence of two models
<img width="1058" height="501" alt="изображение" src="https://github.com/user-attachments/assets/d257eaa1-d411-4ea4-8428-c145aa02fa45" />

# Hypotheses

Hypothesis Testing — Case 19
## 1. Economic imbalance (gold/experience) is a top-3 factor influencing match duration.
## 2 "Turbo" mode as a determining factor.
Feature importance was calculated using a decision tree. Outliers were removed based on the 3-sigma rule.
Sample size: 13,048 matches.
## 3.The tree model outperforms the baseline, and the gap widens over time.
We calculated the MAE and RMSE between the baseline (median) and the tree model across 5-, 10-, and 15-minute intervals.

# Conclusion
Predicting match duration
A Random Forest model was trained on 13,048 matches.
The MAE was 4.28 minutes, and the RMSE was 6.23 minutes.
Economic indicators also proved to be significant features.
For matches lasting longer than 45 minutes, the error is higher — approximately 16.33 minutes.

# Кейс 3. Контр-пики героев (Hero Counter-Picks)

Проанализировать пары героев-соперников и их показатели винрейта.

Оценить эффективность противостояний между героями с учётом следующих факторов:
- Игровая сторона (Radiant / Dire).
- Версия игрового патча.
- Позиция и роль героя.
- Размер выборки.

Необходимо учитывать доверительные интервалы при интерпретации винрейта и не делать выводы исключительно на основе его значений.

## Гипотезы

1. Некоторые герои имеют более высокий винрейт против определённых противников, чем их собственный средний винрейт.
2. Надёжность выявленных контр-пиков зависит от количества сыгранных матчей.
3. У Pudge отсутствуют контр-пики.

## Источники данных

- `pg__matches`
- `pg__match_players`
- `pg__heroes`

## Обработка данных

1. Удаляются записи с отсутствующими значениями (`NULL`) в исходных данных.
2. Исключаются матчи продолжительностью менее 10 минут.
3. Оставляются только матчи в формате 5 на 5.
4. Исключаются записи с отсутствующими значениями (`NULL`) или некорректными значениями `account_id`.
5. Исключаются записи с отсутствующими значениями (`NULL`) или некорректными значениями `hero_id` (меньше или равно 0).
6. Добавляются названия героев и их атрибуты.
7. Исключаются пары героев, которые встречаются менее чем в 30 матчах.

## Методология

На первом этапе создаётся модель PCA (Principal Component Analysis), которая позволяет выявить, насколько часто и в каких игровых контекстах герои встречаются друг с другом, представляя их в общем векторном пространстве.

Далее формируются все возможные пары героев и рассчитывается винрейт каждого героя против соответствующего противника.

На заключительном этапе в качестве входных данных для расчёта доверительных интервалов Уилсона (Wilson Score Interval) используются:
- Векторы героев.
- Винрейт каждой пары героев.
- Количество матчей для каждой пары.

Это позволяет оценить статистическую надёжность полученных показателей и выявить потенциальные контр-пики.

## Результат обработки

`mart_hero_counterpick_stats` — итоговая витрина данных, содержащая информацию о героях и ключевые статистические показатели их противостояний.

Данная таблица была установлена в Linage, после чего все необходимые значения были переданы в Apache Superset для визуализации.

## Визуализация

### Разница между винрейтом Anti-Mage против других героев и его базовым винрейтом

<img width="1072" height="547" alt="изображение" src="https://github.com/user-attachments/assets/f21e91b9-bda4-43bd-b507-d6f245dd881d" />

### Тепловая карта противостояний героев (Heatmap)

<img width="1052" height="612" alt="изображение" src="https://github.com/user-attachments/assets/b16e213f-358d-4115-a1c5-8830f6b989ef" />

### Распределение героев по количеству матчей и отклонению их винрейта

<img width="1085" height="571" alt="изображение" src="https://github.com/user-attachments/assets/1ec2e37d-c5ca-4981-9755-7f2f3d4a8194" />

## Проверка гипотез

### 1. Надёжность выявленных контр-пиков зависит от количества сыгранных матчей.

<badge color="success">Подтверждена</badge>

Чем больше матчей сыграно между двумя героями, тем надёжнее оценка их противостояния. При небольшом размере выборки винрейт может значительно колебаться, поэтому его необходимо рассматривать вместе с доверительным интервалом.

### 2. Выбор контр-пика влияет на винрейт.

<badge color="success">Подтверждена</badge>

Как видно на тепловой карте, наблюдаются выраженные отклонения винрейта для отдельных пар героев. Это указывает на наличие противостояний, в которых выбор конкретного героя может быть связан с преимуществом или недостатком относительно среднего показателя.

### 3. У Pudge отсутствуют жёсткие контр-пики.

<badge color="success">Подтверждена</badge>

Разница между винрейтом Pudge против любого отдельного героя и его базовым винрейтом не превышает 5%.

Таким образом, в рамках выбранного критерия не выявлено героев, которые обеспечивали бы выраженное преимущество против Pudge.

## Выводы

### Анализ контр-пиков героев Dota 2

В ходе исследования были обработаны данные более чем по **309 000 матчам** и **127 героям**.

Основные результаты:

- С помощью PCA и косинусного сходства (Cosine Similarity) рассчитано **8 001 сходство между парами героев**.
- Итоговый набор данных содержит **5 286 направленных противостояний** (Directed Matchups).
- Для каждой пары рассчитаны:
  - **Matchup Win Rate** — винрейт героя против конкретного противника.
  - **Baseline Win Rate** — базовый винрейт героя.
  - **95% Confidence Interval** — 95%-й доверительный интервал.
- Проверки качества данных успешно пройдены: некорректных значений винрейта и дубликатов не обнаружено.

Таким образом, в рамках исследования была построена система анализа противостояний героев, позволяющая оценивать потенциальные контр-пики с учётом статистической надёжности результатов, объёма выборки и различий между игровыми матчапами.

# Case 3
Analyze pairs of rival heroes and their win rates.
Evaluate hero pairs while accounting for the side (Radiant/Dire), game patch, role, and sample size. Do not draw conclusions based on win rates without considering confidence intervals.

## Hypotheses
1. Some heroes have a higher win rate against specific opponents than their own average win rate.
2. The reliability of identified counter-picks depends on the number of matches played.
3. Pudge has no counter-picks.

# Data source
pg__matches
pg__match_players
pg__heroes

# Data Processing
1. Remove `NULL` values ​​from the source data.
2. Exclude matches with a duration of less than 10 minutes.
3. Retain only 5v5 format matches.
4. Exclude records containing `NULL` values ​​or invalid `account_id`s.
5. Exclude records containing `NULL` values ​​or where `hero_id <= 0`.
6. Add hero names and attributes.
7. Exclude hero pairs that appear in fewer than 30 games.

# Methodology
To begin, we create a PCA model that reveals how frequently—and in what contexts—heroes encounter one another within a vector space. Next, we generate pairs of all heroes and calculate their win rates against each other. Finally, we feed the character vectors, win rates, and the number of games played for each hero pair into a Wilson score interval.

# Output
mart_hero_counterpick_stats - which contains all the information about the characters and key statistics, we installed this table on Linage and transfer all the values ​​to the superset

# Visualization
## Win rate differential for Anti-Mage against all other characters
<img width="1072" height="547" alt="изображение" src="https://github.com/user-attachments/assets/f21e91b9-bda4-43bd-b507-d6f245dd881d" />

## Heat map 
<img width="1052" height="612" alt="изображение" src="https://github.com/user-attachments/assets/b16e213f-358d-4115-a1c5-8830f6b989ef" />

## Distribution of characters by number of matches and the deviation in their win rates
<img width="1085" height="571" alt="изображение" src="https://github.com/user-attachments/assets/1ec2e37d-c5ca-4981-9755-7f2f3d4a8194" />

# Hypotheses

## 1.The reliability of the identified counter-picks depends on the number of matches.
Confirmed
## Counter-picking affects win rate. As can be seen on the heatmap, there are spikes in win rate activity.
Confirmed.
## Pudge has no hard counters. (The win-rate differential against any given hero is 5% or less.)
Confirmed.

# Conclusion
Hero Counter-Pick Analysis
Processed over 309,000 matches and 127 heroes.
Calculated 8,001 hero-pair similarities using PCA and Cosine Similarity.
The final dataset contains 5,286 directed matchups.
For each pair, we calculated the Matchup Win Rate, Baseline Win Rate, and 95% confidence intervals.
Quality checks passed: no invalid win rates or duplicates were found.

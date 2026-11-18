# Кейс 1. Первая башня (First Tower)

Исследовать взаимосвязь между временем уничтожения первой башни и вероятностью победы атакующей команды.

Необходимо определить:
- Какая башня была уничтожена первой.
- Какой стороне принадлежала эта башня.
- В какой момент времени произошло уничтожение.

Сопоставить полученное преимущество на ранней стадии игры с итогом матча и последующим контролем карты.

## Гипотезы

1. Если команда уничтожает первую башню, её вероятность победы увеличивается.
2. Влияние раннего уничтожения первой башни T1 на вероятность победы различается для команд Radiant и Dire.
3. Чем раньше команда уничтожает первую башню T1, тем выше её вероятность победы.

## Источники данных

- `mongo__tower_death_events`
- `pg__matches`
- `pg__npcs`

## Обработка данных

1. Загружаются все матчи из `pg__matches`.
2. С помощью `match_id` объединяются данные о событиях из `mongo__tower_death_events`.
3. Оставляются только события с корректной временной меткой, попадающей в интервал продолжительности соответствующего матча.
4. Удаляются дубликаты событий на основе поля `id`.
5. Определяется первое изменение трёх младших битов маски. Эта логика используется для выявления уничтожения башни T1.
6. Определяется сторона, которой принадлежала уничтоженная башня, и команда-соперник, совершившая её уничтожение.
7. Формируется одна запись на каждый матч. Матчи, в которых отсутствуют события уничтожения башен T1, исключаются.
8. Исключаются матчи, в которых башни были уничтожены в одну и ту же секунду.

## Методология

После выполнения всех преобразований данных выполняется группировка матчей по временным интервалам.

Для каждой стороны — Dire и Radiant — рассчитывается средний винрейт (Win Rate, WR) в зависимости от времени уничтожения первой башни.

## Результат обработки

`mart_first_t1_winrate` — итоговая витрина данных, содержащая информацию о винрейте команд в зависимости от времени уничтожения первой башни.

Данная таблица была установлена в Linage, после чего все необходимые значения были переданы в Apache Superset для визуализации.

## Визуализация

**Рисунок 1. Винрейт в зависимости от времени уничтожения первой башни**

<img width="1061" height="503" alt="изображение" src="https://github.com/user-attachments/assets/0a0053fd-c15e-44fb-a6db-635501a257d0" />

**Рисунок 2. Распределение данных по временным интервалам**

<img width="1046" height="558" alt="изображение" src="https://github.com/user-attachments/assets/695bf632-0596-4f9b-ac4b-aeb9e4a02720" />

## Проверка гипотез

### 1. Уничтожение первой башни T1 повышает вероятность победы атакующей команды.

<badge color="success">Подтверждена</badge>

Команда, уничтожившая первую башню T1, имеет более высокую вероятность победы.

### 2. Влияние уничтожения первой башни различается для Radiant и Dire.

<badge color="success">Подтверждена</badge>

Обнаружена разница в показателях винрейта между Radiant и Dire, составляющая приблизительно 5%.

### 3. Чем раньше уничтожена башня T1, тем выше вероятность победы команды.

<badge color="warning">Частично подтверждена</badge>

В целом наблюдается снижение винрейта по мере увеличения времени уничтожения первой башни.

Однако после отметки в 25 минут вероятность победы резко возрастает.

Это объясняется небольшим размером выборки: ситуации, в которых первая башня уничтожается только на 25-й минуте, встречаются редко. Поэтому полученные значения могут быть статистически нестабильными.

## Выводы

### Влияние уничтожения первой башни T1 на вероятность победы

В рамках исследования было проанализировано **313 654 матча**, соответствующих условиям отбора.

Основные результаты:

- Если первая башня уничтожена в течение первых 5 минут, команда, уничтожившая её, побеждает в **65,5% случаев**.
- Если первая башня уничтожена в промежутке с 15-й по 20-ю минуту, винрейт команды составляет **60,6%**.
- Выявлена статистическая взаимосвязь: раннее уничтожение первой башни связано с более высокой вероятностью победы команды, которая её уничтожила.

Таким образом, результаты исследования указывают на то, что получение преимущества в виде уничтожения первой башни на ранней стадии игры положительно связано с итоговым результатом матча.

# Case 1
Examine the relationship between the timing of the first tower's destruction and the attacking side's probability of winning.
Identify the first tower to be destroyed, which side it belonged to, and the time at which the event occurred. Correlate this early advantage with the match outcome and subsequent map control.

## Hypotheses
1. If a team destroys the first tower, their probability of winning increases.
2. The impact of destroying the first T1 tower early on the probability of winning differs between the Radiant and Dire teams.
3. The earlier a team destroys the first T1 tower, the higher their probability of winning.

# Data source
mongo__tower_death_events
pg__matches
pg__npcs

# Data Processing
1. Loads all matches from `pg__matches`.
2. Joins events from `mongo__tower_death_events` using `match_id`.
3. Retains only those events with a valid timestamp within the duration of the match.
4. Removes duplicate events based on their `id`.
5. Identifies the first change in the lowest three bits of the mask—this is the logic used to pinpoint the destruction of a T1 tower.
6. Determines which side’s tower was destroyed and which opposing team destroyed it.
7. Produces one row per match; matches with no T1 tower destruction events are excluded.
8. Excludes matches in which towers were destroyed during the same second.

# Methodology
After all data transformations, we group the data by time ranges and then calculate the average win rate (WR) for each side Dire or Radiant.

# Output
mart_first_t1_winrate - which contains all the information on the win rate, we installed this table on Linage and transfer all the values ​​to the superset

# Visualization
<img width="1061" height="503" alt="изображение" src="https://github.com/user-attachments/assets/0a0053fd-c15e-44fb-a6db-635501a257d0" />
img1 Winrate in terms of time

<img width="1046" height="558" alt="изображение" src="https://github.com/user-attachments/assets/695bf632-0596-4f9b-ac4b-aeb9e4a02720" />
img2 Data distribution over a time range

# Hypotheses

## 1.Destroying the first T1 tower increases the attacking sides chances of victory.
Confirmed
## 2.The impact of the first tower
differs between Radiant and
Dire (a difference of ~5%).
Confirmed.
## 3.The earlier a T1 tower is destroyed, the higher the probability of victory (a difference of >5% between the 5-minute and 25-minute marks).
Despite an overall decline in win rate over time, the chance of winning increases sharply after the 25-minute mark.
This is due to a small sample size, as such an occurrence (the first tower falling at 25 minutes) is rare.

# Conclusion
The Impact of Destroying the First Tier 1 (T1) Tower on Victory
313,654 relevant matches were analyzed.
If the first tower is destroyed within the first 5 minutes, the team that destroys it wins in 65.5% of cases.
When it is destroyed between the 15th and 20th minute, the win rate is 60.6%.
A statistical correlation has been identified: the early destruction of the first tower is associated with a higher probability of victory.

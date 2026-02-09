# 📊 Тимски Проект: Споредба на Алгоритми за Предвидување на Временски Серии

## 👥 Тим

| Име | Индекс |
|-----|--------|
| Амел Махмутовиќ | 203100 |
| Христина Степаноска | 183007 |
| Михаела Николовска | 183014 |

---

## 🎯 Цел на Проектот

Овој проект ги истражува и споредува различни методи за предвидување на временски серии на три реални множества податоци од Kaggle:

1. **📦 Rossmann Store Sales** - Дневна продажба во малопродажба
2. **✈️ Air Passengers** - Месечен број на авио патници (1949-1960)
3. **⚡ Hourly Energy Consumption** - Часовна потрошувачка на електрична енергија

Целта е да се одговори на клучното прашање: **Кој тип на модел работи најдобро за различни видови временски серии?**

---

## 📚 Методологија

### Модели Тестирани

#### 1. Базни Модели (Baseline)
- **Naive**: Последната вредност се користи како предвидување
- **Moving Average**: Просек на претходни вредности
- **Seasonal Naive**: Користи вредност од истиот период претходна година/недела/ден

#### 2. Класични Статистички Модели
- **Prophet** (Facebook): Автоматско моделирање на тренд и сезоналност
- **SARIMA**: Seasonal AutoRegressive Integrated Moving Average

#### 3. Машинско Учење
- **Random Forest**: Ensemble од дрва на одлука
- **XGBoost**: Gradient boosting со оптимизации
- **LightGBM**: Брз gradient boosting алгоритам

### Метрики за Евалуација

| Метрика | Опис | Кога е Важна |
|---------|------|--------------|
| **RMSE** | Root Mean Squared Error | Чувствителна на големи грешки |
| **MAE** | Mean Absolute Error | Просечна апсолутна грешка |
| **MAPE** | Mean Absolute Percentage Error | Процентуална грешка, лесна за интерпретација |

**MAPE** е главната метрика за споредба бидејќи е scale-independent и лесна за разбирање.

---

## 📊 Резултати по Датасет

### 1. 📦 Rossmann Store Sales

**Карактеристики:**
- Дневни податоци (942 дена)
- Силна неделна сезоналност
- Влијание на промоции и празници
- Голема варијабилност меѓу продавници

**Најдобар Модел:** Random Forest
- MAPE: 6.27%
- RMSE: 488,790
- MAE: 378,878

**Клучни Наоди:**
- ML моделите значително подобри од класичните (6-9% MAPE vs 15% за Prophet)
- Naive модел катастрофално лош (300% MAPE) 
- Feature engineering (lag features, промоции, празници) критично важен
- Неделна сезоналност јасно изразена

**Рангирање:**
1. 🥇 Random Forest (6.27%)
2. 🥈 XGBoost (7.81%)
3. 🥉 LightGBM (9.18%)
4. Prophet (14.77%)
5. SARIMA (N/A - не е тестиран)

---

### 2. ✈️ Air Passengers

**Карактеристики:**
- Месечни податоци (144 месеци)
- Линеарен тренд со пораст
- Силна годишна сезоналност (летни пикови)
- Класично множество за time series

**Најдобар Модел:** Prophet
- MAPE: 6.54%
- RMSE: 40.39
- MAE: 31.17

**Клучни Наоди:**
- Prophet одличен за податоци со јасна сезоналност и тренд
- ML моделите (7-12% MAPE) малку послаби од Prophet
- SARIMA солиден (14.7% MAPE) но побавен
- Простиот MA-12 има 15.5% MAPE - добра baseline

**Рангирање:**
1. 🥇 Prophet (6.54%)
2. 🥈 Random Forest (7.22%)
3. 🥉 XGBoost (10.48%)
4. LightGBM (12.28%)
5. SARIMA (14.70%)

---

### 3. ⚡ Hourly Energy Consumption

**Карактеристики:**
- Часовни податоци (мултиплицирани наблудувања)
- Дневна сезоналност (24-часовен циклус)
- Неделна сезоналност (работни денови vs викенди)
- Брзи промени и пикови

**Најдобар Модел:** LightGBM
- MAPE: 0.80%
- RMSE: 395.76
- MAE: 305.22

**Клучни Наоди:**
- Сите ML модели имаат исклучително ниски грешки (<1% MAPE!)
- Random Forest, XGBoost и LightGBM практично идентични
- Prophet солиден (9.8%) но не може да се натпреварува со ML
- Lag features критично важни за часовни податоци

**Рангирање:**
1. 🥇 LightGBM (0.80%)
2. 🥈 Random Forest (0.81%)
3. 🥉 XGBoost (0.89%)
4. Prophet (9.80%)
5. Seasonal Naive (10.20%)

---

## 🏆 Генерални Заклучоци

### 1. Кој Модел е Најдобар?

**Нема еден "најдобар" модел - зависи од податоците:**

| Тип на Податоци | Препорака | Причина |
|-----------------|-----------|---------|
| **Јасна сезоналност + тренд** | Prophet | Автоматско детектирање, брзо, добро |
| **Комплексни шаблони** | XGBoost/LightGBM | Најдобра точност |
| **Многу features** | Random Forest | Робустен, лесен за tuning |
| **Брзо прототипирање** | Prophet | Минимален feature engineering |
| **Production со голем scale** | LightGBM | Брзо, ниска меморија |

### 2. ML vs Класични Модели

**Machine Learning (RF, XGBoost, LightGBM):**
- ✅ Повисока точност кога има доволно податоци
- ✅ Автоматско учење на комплексни шаблони
- ✅ Можност за користење на многу features
- ❌ Потребен feature engineering (lags, rolling stats)
- ❌ Потешко за интерпретација

**Класични (Prophet, SARIMA):**
- ✅ Експлицитно моделирање на сезоналност
- ✅ Добра интерпретација
- ✅ Минимален feature engineering
- ✅ Добри за податоци со јасна структура
- ❌ Послаби на комплексни, нелинеарни односи

### 3. Важноста на Feature Engineering

За ML моделите, **квалитетот на features е критичен:**

```python
# Најважни features за time series:
1. Lag features (претходни вредности)
2. Rolling statistics (подвижни просеци, std)
3. Time features (час, ден, месец, година)
4. Calendar features (празници, викенди)
5. Domain-specific features (промоции, weather, итн)
```

**Пример:** На Rossmann, додавањето на lag features ја подобри точноста од 15% MAPE (Prophet) на 6% MAPE (Random Forest).

### 4. Сложеност на Датасети

**Ранкирање по тежина (по просечна MAPE):**

1. 🟢 **Energy** (просек: ~5%) - Најлесен
   - Предвидлива дневна сезоналност
   - Стабилни шаблони
   
2. 🟡 **Air Passengers** (просек: ~12%) - Среден
   - Јасна структура
   - Мала сезонска варијација
   
3. 🔴 **Rossmann** (просек: ~60%) - Најтежок
   - Висока варијабилност
   - Многу надворешни фактори
   - Различни шаблони по продавници

### 5. Практични Препораки

#### За Бизнис Апликации:
```
1. Започни со Prophet за брз baseline
2. Ако Prophet дава >10% MAPE, пробај ML модели
3. Користи ensemble (Prophet + XGBoost)
4. Секогаш тествај Naive модел како проверка
```

#### За Production:
```
1. LightGBM за брзина и ниска меморија
2. Сними го моделот и features pipeline
3. Мониторирај перформанси со текот на времето
4. Re-train периодично со нови податоци
```

#### За Истражување:
```
1. Тествај повеќе модели
2. Користи cross-validation
3. Анализирај грешки (кога моделот греши?)
4. Experiment со различни features
```

---

## 🔬 Технички Детали

### Подготовка на Податоци

```python
# Општ pipeline:
1. Вчитување и валидација
2. Проверка за missing values
3. Конверзија на datetime
4. Создавање на features
5. Train/test split (80/20 или последен период)
```

### Hyperparameters

**Random Forest:**
```python
n_estimators = 100
max_depth = 15
random_state = 42
```

**XGBoost:**
```python
n_estimators = 300
learning_rate = 0.05
max_depth = 6
```

**LightGBM:**
```python
n_estimators = 300
learning_rate = 0.05
max_depth = 6
```

**Prophet:**
```python
yearly_seasonality = True
weekly_seasonality = True
daily_seasonality = True  # за часовни податоци
```

---

## 📈 Споредбени Графикони

### MAPE Across Datasets
![MAPE Comparison](comparison_mape_by_dataset_FILTERED.png)

### Best Model Performance
![Best Models](best_models_comparison_IMPROVED.png)

### Model Category Performance
![Categories](model_category_comparison.png)

---

## 💡 Научени Лекции

1. **Не постои универзален модел** - Секој датасет е различен
2. **Baseline моделите се важни** - За проверка дали комплексноста има смисла
3. **Feature engineering > Model choice** - Добри features се покритични од изборот на модел
4. **Domain knowledge помага** - Знаењето за проблемот помага во создавање features
5. **Prophet е одличен starter** - Брз, лесен, често доволно добар
6. **ML моделите бараат повеќе податоци** - Но кога имаш, се подобри

---

## 🔄 Идни Подобрувања

1. **Додај повеќе модели:**
   - LSTM (Deep Learning)
   - Facebook's NeuralProphet
   - AutoML solutions

2. **Hyperparameter tuning:**
   - Grid Search / Random Search
   - Bayesian Optimization

3. **Ensemble методи:**
   - Stacking
   - Weighted averaging

4. **Feature selection:**
   - Feature importance analysis
   - Remove redundant features

5. **Cross-validation:**
   - Time series cross-validation
   - Multiple splits за поробустна евалуација

---

## 📚 Референци

1. **Datasets:**
   - [Rossmann Store Sales - Kaggle](https://www.kaggle.com/c/rossmann-store-sales)
   - [Air Passengers Dataset](https://www.kaggle.com/datasets/rakannimer/air-passengers)
   - [Hourly Energy Consumption](https://www.kaggle.com/datasets/robikscube/hourly-energy-consumption)

2. **Libraries Used:**
   - Prophet: Facebook's time series library
   - XGBoost: Extreme Gradient Boosting
   - LightGBM: Light Gradient Boosting Machine
   - scikit-learn: Machine Learning in Python
   - statsmodels: Statistical models for Python

3. **Методологија:**
   - Hyndman, R.J., & Athanasopoulos, G. (2021). *Forecasting: principles and practice*
   - Taylor, S.J., & Letham, B. (2018). *Forecasting at scale*

---

## 🎓 Заклучок

Овој проект покажа дека **изборот на модел зависи од природата на временската серија**. Додека Prophet е одличен за податоци со јасна сезоналност и тренд (Air Passengers), ML моделите се супериорни за комплексни, high-frequency податоци (Energy) и за ситуации со многу надворешни фактори (Rossmann).

**Главната порака:** Секогаш тествај повеќе модели, започни со едноставен baseline, и инвестирај време во feature engineering пред да одбереш сложен модел.

---

**Датум:** Јануари 2025  
**Курс:** Машинско Учење / Временски Серии  
**Институција:** [Вашата институција]


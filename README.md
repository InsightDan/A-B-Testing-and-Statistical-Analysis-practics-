# Описание кейса
Эту работу я выполнил для отработки навыков статистического анализа и A/B тестирования.
В моём распоряжении было несколько датасетов:

* Эксперимент с 3 группами (A, B, C) и 30 000 пользователей

* A/B-тест с сегментацией по уровню активности (high/low) на 100 000 пользователей

* Классические наборы данных (tips, boston housing) для тренировки

* Дополнительный эксперимент control vs experimental на 1 254 наблюдениях

### Мои задачи

* Провести однофакторный дисперсионный анализ (ANOVA) для сравнения 3 групп

* Выполнить множественные сравнения (post-hoc тесты) между всеми парами групп

* Проанализировать двухфакторный эксперимент (группа × сегмент)

* Проверить предпосылки статистических тестов (нормальность, гомогенность дисперсий)

* Интерпретировать результаты и сделать выводы о статистической значимости

### Инструменты, которые я использовал

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)  
![Jupyter Notebook](https://img.shields.io/badge/jupyter-%23F37626?style=for-the-badge&logo=jupyter&logoColor=white)  
![Pandas](https://img.shields.io/badge/pandas-%23150458?style=for-the-badge&logo=pandas&logoColor=white)  
![NumPy](https://img.shields.io/badge/numpy-%23013243?style=for-the-badge&logo=numpy&logoColor=white)  
![SciPy](https://img.shields.io/badge/SciPy-%230C55A5?style=for-the-badge&logo=scipy&logoColor=white)  
![Statsmodels](https://img.shields.io/badge/statsmodels-3B3B98?style=for-the-badge&logo=statsmodels&logoColor=white)  
![Pingouin](https://img.shields.io/badge/Pingouin-FF6B6B?style=for-the-badge&logo=python&logoColor=white)  
![Seaborn](https://img.shields.io/badge/seaborn-3776AB?style=for-the-badge&logo=seaborn&logoColor=white)  
![Matplotlib](https://img.shields.io/badge/matplotlib-%23ffffff?style=for-the-badge&logo=matplotlib&logoColor=black)


### Что я делал пошагово

* Загрузил и изучил структуру данных:
df1 = pd.read_csv('experiment_3groups.csv') # 30k строк, группы A/B/C
df2 = pd.read_csv('ab_test_segments.csv') # 100k строк, test/control × high/low

* Проверил нормальность распределений по группам:
normality = df1.groupby('group')['events'].apply(lambda x: pg.normality(x))

* Провёл однофакторный ANOVA для групп A, B, C:
f_stat, p_value = ss.f_oneway(group_A, group_B, group_C)

* Выполнил post-hoc сравнения (Tukey HSD):
tukey_results = pairwise_tukeyhsd(df1['events'], df1['group'])

* Двухфакторный анализ для группы и сегмента:
model = smf.ols('events ~ C(group) * C(segment)', data=df2).fit()
anova_results = anova_lm(model)

* Построил визуализации: boxplot, гистограммы, QQ-plots для проверки предпосылок

### Мои результаты

ANOVA показал статистически значимые различия между группами (p < 0.001)

Post-hoc тесты: все пары групп значимо отличаются друг от друга

Двухфакторный анализ: обнаружил взаимодействие группы и сегмента

Размеры эффектов: рассчитаны Cohen’s d и η² для оценки практической значимости

Нормальность: в большинстве групп распределения близки к нормальным

Практическая ценность
Эта работа помогла мне освоить полный цикл статистического анализа экспериментов: от проверки предпосылок до интерпретации результатов. Навыки применимы в продуктовой аналитике, маркетинговых исследованиях и A/B-тестировании.

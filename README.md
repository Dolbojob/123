# 123
```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns


df = pd.read_csv('Global_Cybersecurity_Threats_2015-2024.csv')

sns.set_style("whitegrid")
plt.rcParams['figure.figsize'] = [12, 6]
plt.rcParams['font.size'] = 10


plt.figure(figsize=(14, 6))
sns.boxplot(data=df, x='Attack Type', y='Financial Loss (in Million $)',
            palette='Set2', order=df.groupby('Attack Type')['Financial Loss (in Million $)'].median().sort_values(ascending=False).index)
plt.title('Распределение финансовых потерь по типам кибератак (2015-2024)', fontsize=14, fontweight='bold')
plt.xlabel('Тип атаки')
plt.ylabel('Финансовые потери (млн $)')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()


country_counts = df['Country'].value_counts().head(10)  # Топ-10 стран

plt.figure(figsize=(14, 6))
bars = plt.bar(country_counts.index, country_counts.values, color=sns.color_palette('viridis', len(country_counts)))
plt.title('Топ-10 стран по количеству зафиксированных кибератак (2015-2024)', fontsize=14, fontweight='bold')
plt.xlabel('Страна')
plt.ylabel('Количество инцидентов')
plt.xticks(rotation=45, ha='right')


for bar in bars:
    height = bar.get_height()
    plt.text(bar.get_x() + bar.get_width()/2., height, f'{int(height)}',
             ha='center', va='bottom', fontsize=9)
plt.tight_layout()
plt.show()


plt.figure(figsize=(14, 6))
scatter = plt.scatter(df['Number of Affected Users'], df['Financial Loss (in Million $)'],
                      c=df['Year'], cmap='plasma', alpha=0.6, s=50, edgecolors='black')
plt.title('Зависимость финансовых потерь от количества затронутых пользователей', fontsize=14, fontweight='bold')
plt.xlabel('Количество затронутых пользователей')
plt.ylabel('Финансовые потери (млн $)')
plt.xscale('log')  # Логарифмическая шкала для лучшей визуализации
plt.colorbar(scatter, label='Год')
plt.tight_layout()
plt.show()


print("📊 Сводная статистика по датасету:")
print(f"• Всего записей: {len(df)}")
print(f"• Период: {df['Year'].min()}–{df['Year'].max()}")
print(f"• Уникальных стран: {df['Country'].nunique()}")
print(f"• Типов атак: {df['Attack Type'].nunique()}")
print(f"• Средние финансовые потери: ${df['Financial Loss (in Million $)'].mean():.2f} млн")
print(f"• Медиана затронутых пользователей: {df['Number of Affected Users'].median():,}")
```

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np


df = pd.read_csv('global_water_consumption_2000_2025.csv')


sns.set_style("whitegrid")
sns.set_palette("husl")
plt.rcParams['figure.figsize'] = [12, 8]


russia_data = df[df['Country'] == 'Russia'].copy()

plt.figure(figsize=(10, 6))
plt.scatter(russia_data['Year'], russia_data['Total Water Consumption (Billion m3)'],
            color='#2E86AB', s=60, alpha=0.8, edgecolors='black')
plt.plot(russia_data['Year'], russia_data['Total Water Consumption (Billion m3)'],
         color='#2E86AB', alpha=0.3, linewidth=1)
plt.xlabel('Год', fontsize=12)
plt.ylabel('Потребление воды (млрд м³)', fontsize=12)
plt.title('Потребление воды в России (2000–2025)', fontsize=14, fontweight='bold')
plt.grid(True, alpha=0.3, linestyle='--')
plt.tight_layout()
plt.show()


plt.figure(figsize=(10, 6))
order = ['Low', 'Moderate', 'High', 'Critical']
sns.boxplot(x='Water Scarcity Level', y='Groundwater Depletion Rate (%)',
            data=df, order=order, palette='RdYlGn_r', showfliers=False)
plt.xlabel('Уровень водного дефицита', fontsize=12)
plt.ylabel('Скорость истощения грунтовых вод (%)', fontsize=12)
plt.title('Распределение истощения грунтовых вод по уровням дефицита воды', fontsize=14, fontweight='bold')
plt.grid(True, alpha=0.3, axis='y', linestyle='--')
plt.tight_layout()
plt.show()


latest_year = df[df['Year'] == 2025].copy()
top_15 = latest_year.nlargest(15, 'Total Water Consumption (Billion m3)')

plt.figure(figsize=(12, 7))
colors = plt.cm.viridis(np.linspace(0.3, 0.9, len(top_15)))
bars = plt.barh(top_15['Country'], top_15['Total Water Consumption (Billion m3)'],
                color=colors, edgecolor='black', linewidth=0.5)
plt.xlabel('Потребление воды (млрд м³)', fontsize=12)
plt.ylabel('Страна', fontsize=12)
plt.title('Топ-15 стран по потреблению воды в 2025 году', fontsize=14, fontweight='bold')
plt.gca().invert_yaxis()  # Самые высокие значения сверху
plt.grid(True, alpha=0.3, axis='x', linestyle='--')
plt.tight_layout()
plt.show()


print("=== Краткая статистика ===")
print(f"Всего стран в датасете: {df['Country'].nunique()}")
print(f"Период данных: {df['Year'].min()} – {df['Year'].max()}")
print(f"\nРаспределение по уровням водного дефицита (2025):")
print(df[df['Year']==2025]['Water Scarcity Level'].value_counts())
```

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns


sns.set_style("whitegrid")
plt.rcParams['figure.figsize'] = [14, 8]
plt.rcParams['font.size'] = 12


file_path = 'Consumption of alcoholic beverages 2017-2023 (Pivot table).csv'

try:
    # Пробуем кодировку cp1251 (стандарт для Windows/Russia)
    df = pd.read_csv(file_path, encoding='cp1251')
except UnicodeDecodeError:
    # Если не подошло, пробуем latin1 (она читает всё, но может искажать некоторые символы)
    print("Кодировка cp1251 не подошла, пробуем latin1...")
    df = pd.read_csv(file_path, encoding='latin1')

print("Данные успешно загружены!")
print(df.head())
print(f"Доступные типы напитков: {df['Type of alcoholic beverages'].unique()}")


plt.figure(figsize=(14, 8))
# Фильтруем данные для наглядности (убираем возможные пустые значения)
plot_data = df.dropna(subset=['Consumption of alcoholic beverages (in liters of pure alcohol per capita)'])

sns.boxplot(x='Type of alcoholic beverages',
            y='Consumption of alcoholic beverages (in liters of pure alcohol per capita)',
            data=plot_data,
            palette='Set2',
            order=plot_data.groupby('Type of alcoholic beverages')['Consumption of alcoholic beverages (in liters of pure alcohol per capita)'].median().sort_values(ascending=False).index)

plt.title('Распределение потребления чистого алкоголя (л на душу населения) по типам напитков\n(2017–2023)', fontsize=16, fontweight='bold')
plt.xlabel('Тип алкогольного напитка', fontsize=14)
plt.ylabel('Потребление чистого алкоголя (л/чел)', fontsize=14)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()


region_avg = df.groupby('Region')['Consumption of alcoholic beverages (in liters of pure alcohol per capita)'].mean().reset_index()
region_avg = region_avg.sort_values(by='Consumption of alcoholic beverages (in liters of pure alcohol per capita)', ascending=False).head(15)

plt.figure(figsize=(14, 8))
sns.barplot(x='Consumption of alcoholic beverages (in liters of pure alcohol per capita)',
            y='Region',
            data=region_avg,
            palette='viridis')

plt.title('Топ-15 регионов РФ по среднему потреблению чистого алкоголя (все напитки)\n(2017–2023)', fontsize=16, fontweight='bold')
plt.xlabel('Среднее потребление чистого алкоголя (л/чел)', fontsize=14)
plt.ylabel('Регион', fontsize=14)
plt.tight_layout()
plt.show()


scatter_types = ['Beer', 'Vodka']
scatter_data = df[df['Type of alcoholic beverages'].isin(scatter_types)].copy()

plt.figure(figsize=(14, 8))
sns.scatterplot(x='Year',
                y='Consumption of alcoholic beverages (in liters of pure alcohol per capita)',
                hue='Type of alcoholic beverages',
                data=scatter_data,
                style='Type of alcoholic beverages',
                s=100,
                alpha=0.7,
                palette={'Beer': '#F4D03F', 'Vodka': '#3498DB'})

plt.title('Изменение потребления чистого алкоголя из Пива и Водки во времени\n(по всем регионам)', fontsize=16, fontweight='bold')
plt.xlabel('Год', fontsize=14)
plt.ylabel('Потребление чистого алкоголя (л/чел)', fontsize=14)
plt.legend(title='Тип напитка', fontsize=12)
plt.xticks([2017, 2018, 2019, 2020, 2021, 2022, 2023])
plt.tight_layout()
plt.show()
```

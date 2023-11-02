import pandas as pd

df = pd.read_csv('countries of the world.csv')
print(df.info())
print(df.head())
print(df.tail())
#Показатель ВВП зависит от занятости населения
#Показатель ВВП не зависит от типа экономики
#Уровень миграции зависит от есть ли выхода к океану

def region_apply(region):
    return region.strip()
df['Region'] = df['Region'].apply(region_apply)

print(df['GDP ($ per capita)'].value_counts())
print(df['GDP ($ per capita)'].mean())
print(df['GDP ($ per capita)'].median())
print(df.describe())

df = df.dropna()


def death_rate_apply(death_rate):
    death_rate = death_rate.replace(',', '.')
    return float(death_rate)
df['Deathrate'] = df['Deathrate'].apply(death_rate_apply)


def birth_rate_apply(birth_rate):
    birth_rate = birth_rate.replace(',','.')
    return float(birth_rate)
df['Birthrate'] = df['Birthrate'].apply(birth_rate_apply)


low_GDP_br = df[df['GDP ($ per capita)']<=1800]['Birthrate'].mean()
low_GDP_br = df[df['GDP ($ per capita)']<=1800]['Deathrate'].mean()


medium_GDP_br = df[(df['GDP ($ per capita)'])>1800 & (df['GDP ($ per capita)']<12950)]['Birthrate'].mean()
medium_GDP_br = df[(df['GDP ($ per capita)'])>1800 & (df['GDP ($ per capita)']<12950)]['Deathrate'].mean()


high_GDP_br = df[(df['GDP ($ per capita)'])>=12950]['Birthrate'].mean()
high_GDP_br = df[(df['GDP ($ per capita)'])>=12950]['Deathrate'].mean()


print('Для низкого ВВП:', 'Смертность:', round(low_GDP_br,2), 'Рождаемость:', round(low_GDP_br,2))
print('Для среднего ВВП;', 'Смертность:', round(medium_GDP_br,2), 'Рождаемость:', round(medium_GDP_br,2))
print('Для высокого ВВП','Смертность:', round(high_GDP_br,2), 'Рождаемость:', round(high_GDP_br,2))


# Проверим, зависит ли уровень миграции от наличия выхода к океану
medium_ocean_migration = df[df['Coastline (coast/area ratio)'] > 0]['Net migration'].mean()
medium_migration = df[df['Coastline (coast/area ratio)'] == 0]['Net migration'].mean()

print('Средний уровень миграции для стран с выходом к океану:', round(medium_ocean_migration, 2))
print('Средний уровень миграции для стран без выхода к океану:', round(medium_migration, 2))

migration_ocean = df[df['Coastline (coast/area ratio)'] > 0]['Net migration']
migration = df[df['Coastline (coast/area ratio)'] == 0]['Net migration']

stat, value= ind(migration_ocean ,migration , equal_var = False)

if  value < 0.05:
    print("Разница в уровне миграции является заметной, подтверждает гипотезу.")
else:
    print("Нет разницы в уровне миграции, не подтверждает гипотезу.")

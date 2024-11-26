import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
sports_data=pd.read_csv('2021-2022 NBA Player Stats - Playoffs.csv',  encoding='latin1', delimiter=';')
sports_data
sports_data.describe
sports_data.shape
sports_data.info
sports_data.head(10)

missing_values = sports_data.isnull().sum()
print(missing_values)

sports_data.fillna(sports_data.median(numeric_only=True), inplace=True)

sports_data.rename(columns={'G': 'Games_Played', 'PTS': 'Points'}, inplace=True)

sports_data.drop_duplicates(inplace=True)

high_minutes_players = sports_data[sports_data['MP'] > 20]

sports_data

scoring_stats = sports_data[['Points', 'FG%', '3P%', 'FT%']].describe()
print(scoring_stats)
top_scorers = sports_data.nlargest(10, 'Points')[['Player', 'Points']]
print(top_scorers)


position_counts = sports_data['Pos'].value_counts()
print(position_counts)



avg_points = np.mean(sports_data['Points'])
print(f"Average Points per Game: {avg_points}")

above_avg_scorers = sports_data[sports_data['Points'] > avg_points]

plt.hist(sports_data['Points'], bins=20, color='blue', edgecolor='black')
plt.title("Distribution of Points")
plt.xlabel("Points")
plt.ylabel("Frequency")
plt.show()

sns.scatterplot(data=sports_data, x='MP', y='Points', hue='Pos')
plt.title("Minutes Played vs Points Scored")
plt.xlabel("Minutes Played")
plt.ylabel("Points Scored")
plt.show()
sns.boxplot(data=sports_data, x='Pos', y='Points')
plt.title("Points by Player Position")
plt.show()

sns.lineplot(data=sports_data, x='Games_Played', y='Points', hue='Pos', ci=None)
plt.title("Points Across Games Played")
plt.show()

sports_data.rename(columns={'PTS': 'Points'}, inplace=True)
top_5_players = sports_data.nlargest(5, 'Points')[['Player', 'Points']]


plt.figure(figsize=(10, 6))
sns.barplot(data=top_5_players, x='Player', y='Points', palette='viridis')

plt.title('Top 5 Players by Points', fontsize=16)
plt.xlabel('Player', fontsize=14)
plt.ylabel('Points', fontsize=14)
plt.xticks(rotation=45, fontsize=12)
plt.tight_layout()

plt.show()

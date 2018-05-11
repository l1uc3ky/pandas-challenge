

```python
#import dependencies 
import pandas as pd
import numpy as np
import json 
```


```python
#import data files 
data_file = 'purchase_data.json'
purchase_data = pd.read_json(data_file, orient="records")
purchase_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>



# Player Counts & Demographics


```python
# obtain player demographics 
player_demos = purchase_data.loc[:, ["Gender", "SN", "Age"]]
player_demos.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Gender</th>
      <th>SN</th>
      <th>Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>Aelalis34</td>
      <td>38</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>Eolo46</td>
      <td>21</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Male</td>
      <td>Assastnya25</td>
      <td>34</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>Pheusrical25</td>
      <td>21</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Male</td>
      <td>Aela59</td>
      <td>23</td>
    </tr>
  </tbody>
</table>
</div>




```python
# obtain unique values of player demographics to obtain total players
player_demos = player_demos.drop_duplicates()
total_players = player_demos.count()[0]
total_players
```




    573




```python
total_playersdf = pd.DataFrame({"Total Players": [total_players]})
```


```python
# Counts and percentages of Male and Female Players
# counts by gender
gender_count = player_demos["Gender"].value_counts()
gender_count

# Percentage of male and female players
gender_percent = (gender_count / total_players)*100
gender_percent

# Put into one table 
gender_demos = pd.DataFrame({"Gender Count": gender_count,
                            "Gender Percentage": gender_percent})

# Round to 2 decimal places 
gender_demos = gender_demos.round(2)


# print data
gender_demos
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Gender Count</th>
      <th>Gender Percentage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>465</td>
      <td>81.15</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>100</td>
      <td>17.45</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>8</td>
      <td>1.40</td>
    </tr>
  </tbody>
</table>
</div>



# Purchase Analysis (Total) 


```python
#Calculations for analysis
#average purchase price 
average_item_price = purchase_data["Price"].mean()

#total number of purchases 
purchase_count = purchase_data["Price"].count()

#total revenue 
total_purchase_value = purchase_data["Price"].sum()

#total items purchased
item_count = len(purchase_data["Item ID"].unique())

# Create a data frame for purchase data analysis 
purchase_totals = pd.DataFrame({"Number of Unique Items": [item_count], 
                               "Total Revenue": [total_purchase_value],
                               "Number of purchases": [purchase_count], 
                               "Average Purchase Price": [average_item_price]})

#summary of purchase analysis 
purchase_totals = purchase_totals.round(2)
purchase_totals["Average Purchase Price"] = purchase_totals["Average Purchase Price"].map("${:,.2f}".format)
purchase_totals["Total Revenue"] = purchase_totals["Total Revenue"].map("${:,.2f}".format)
purchase_totals
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Number of Unique Items</th>
      <th>Number of purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.93</td>
      <td>183</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>



# Purchase Analysis (Gender)  


```python
#Calculations for purchase analysis by gender 
gender_purchases = purchase_data.groupby(["Gender"]).sum()["Price"].rename("Total Purchase Value")
gender_avgprice = purchase_data.groupby(["Gender"]).mean()["Price"].rename("Average Purchase Value")
gender_counts = purchase_data.groupby(["Gender"]).count()["Price"].rename("Purchase Count")

#Normalize data 
normalized_total = gender_purchases / gender_count

# Create data frame to house results 
gender_data = pd.DataFrame({"Normalized Total": normalized_total, 
                            "Purchase Count": gender_counts, 
                            "Total Purchase Value": gender_purchases, 
                            "Average Purchase Value": gender_avgprice})

#format results 
gender_data = gender_data.round(2)
gender_data["Average Purchase Value"] = gender_data["Average Purchase Value"].map("${:,.2f}".format)
gender_data["Total Purchase Value"] = gender_data["Total Purchase Value"].map("${:,.2f}".format)
gender_data["Normalized Total"] = gender_data["Normalized Total"].map("${:,.2f}".format)


#Print results of purchase analysis 
gender_data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Value</th>
      <th>Normalized Total</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>$2.82</td>
      <td>$3.83</td>
      <td>136</td>
      <td>$382.91</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>$2.95</td>
      <td>$4.02</td>
      <td>633</td>
      <td>$1,867.68</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>$3.25</td>
      <td>$4.47</td>
      <td>11</td>
      <td>$35.74</td>
    </tr>
  </tbody>
</table>
</div>



# Age Demographics


```python
age_bins = [0, 9.90, 14.90, 19.90, 24.9, 29.9, 34.90, 39.90, 9999999]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", ">40"]

#Cut data to put players into age bins 
#create new column to add the series in 
purchase_data["Age Ranges"] = pd.cut(purchase_data["Age"], age_bins, labels=group_names)
purchase_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>Age Ranges</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
      <td>35-39</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
      <td>30-34</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
      <td>20-24</td>
    </tr>
  </tbody>
</table>
</div>




```python
# total players and percentages by age 
age_demos_total = purchase_data["Age Ranges"].value_counts()
age_demo_percents = (age_demos_total / total_players) * 100

#create data frame to hold the results 
age_demos = pd.DataFrame({"Total Count": age_demos_total, "Percent of Players": age_demo_percents})
age_demos = age_demos.sort_index()
age_demos = age_demos.round(2)
age_demos
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percent of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>4.89</td>
      <td>28</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>6.11</td>
      <td>35</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>23.21</td>
      <td>133</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>58.64</td>
      <td>336</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>21.82</td>
      <td>125</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>11.17</td>
      <td>64</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>7.33</td>
      <td>42</td>
    </tr>
    <tr>
      <th>&gt;40</th>
      <td>2.97</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>



# Purchase Analysis (Age)


```python
#Calculations for purchase analysis by age bins  
age_purchases = purchase_data.groupby(["Age Ranges"]).sum()["Price"].rename("Total Purchase Value")
age_avgprice = purchase_data.groupby(["Age Ranges"]).mean()["Price"].rename("Average Purchase Value")
age_counts = purchase_data.groupby(["Age Ranges"]).count()["Price"].rename("Purchase Count")

#Normalize data 
normalized_total = age_purchases / age_demos_total

# Create data frame to house results 
age_data = pd.DataFrame({"Normalized Total": normalized_total, 
                            "Purchase Count": age_counts, 
                            "Total Purchase Value": age_purchases, 
                            "Average Purchase Value": age_avgprice})

#format results 
age_data = age_data.round(2)
age_data["Average Purchase Value"] = age_data["Average Purchase Value"].map("${:,.2f}".format)
age_data["Total Purchase Value"] = age_data["Total Purchase Value"].map("${:,.2f}".format)
age_data["Normalized Total"] = age_data["Normalized Total"].map("${:,.2f}".format)


#Print results of purchase analysis 
age_data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Value</th>
      <th>Normalized Total</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10-14</th>
      <td>$2.77</td>
      <td>$2.77</td>
      <td>35</td>
      <td>$96.95</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>$2.91</td>
      <td>$2.91</td>
      <td>133</td>
      <td>$386.42</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>$2.91</td>
      <td>$2.91</td>
      <td>336</td>
      <td>$978.77</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>$2.96</td>
      <td>$2.96</td>
      <td>125</td>
      <td>$370.33</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>$3.08</td>
      <td>$3.08</td>
      <td>64</td>
      <td>$197.25</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>$2.84</td>
      <td>$2.84</td>
      <td>42</td>
      <td>$119.40</td>
    </tr>
    <tr>
      <th>&lt;10</th>
      <td>$2.98</td>
      <td>$2.98</td>
      <td>28</td>
      <td>$83.46</td>
    </tr>
    <tr>
      <th>&gt;40</th>
      <td>$3.16</td>
      <td>$3.16</td>
      <td>17</td>
      <td>$53.75</td>
    </tr>
  </tbody>
</table>
</div>



# Top 5 Spenders 


```python
# Identify the top 5 spenders in the game by total purchase value, then list in a table: 
# group totals, averages, and counts by SN 
user_total = purchase_data.groupby(["SN"]).sum()["Price"].rename("Total Purchase Amount")
user_average = purchase_data.groupby(["SN"]).mean()["Price"].rename("Average Purchase Price")
user_count = purchase_data.groupby(["SN"]).count()["Price"].rename("Purchase Count")

#Create data frame to hold results 
user_spend = pd.DataFrame({"Total Purchase Amount": user_total,
                          "Average Purchase Price": user_average,
                           "Purchase Count": user_count})

#format results 
user_spend = user_spend.round(2)
user_spend["Average Purchase Price"] = user_spend["Average Purchase Price"].map("${:,.2f}".format)
user_spend["Total Purchase Amount"] = user_spend["Total Purchase Amount"].map("${:,.2f}".format)

#Print results of purchase analysis 
#sort values to obtain top 5 
user_spend.sort_values("Total Purchase Amount", ascending=False).head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Amount</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Qarwen67</th>
      <td>$2.49</td>
      <td>4</td>
      <td>$9.97</td>
    </tr>
    <tr>
      <th>Sondim43</th>
      <td>$3.13</td>
      <td>3</td>
      <td>$9.38</td>
    </tr>
    <tr>
      <th>Tillyrin30</th>
      <td>$3.06</td>
      <td>3</td>
      <td>$9.19</td>
    </tr>
    <tr>
      <th>Lisistaya47</th>
      <td>$3.06</td>
      <td>3</td>
      <td>$9.19</td>
    </tr>
    <tr>
      <th>Tyisriphos58</th>
      <td>$4.59</td>
      <td>2</td>
      <td>$9.18</td>
    </tr>
  </tbody>
</table>
</div>



# Most Popular Items


```python
# Identify the top 5 popular items in the game by purchase count, then list in a table: 
# group totals, averages, and counts by SN 
pop_total = purchase_data.groupby(["Item ID", "Item Name"]).sum()["Price"].rename("Total Purchase Value")
popular_count = purchase_data.groupby(["Item ID", "Item Name"]).count()["Price"].rename("Purchase Count")

#Create data frame to hold results 
popular_items = pd.DataFrame({"Total Purchase Value": pop_total, 
                              "Purchase Count": popular_count})

#format results 
popular_items = popular_items.round(2)
popular_items["Total Purchase Value"] = popular_items["Total Purchase Value"].map("${:,.2f}".format)

#Print results of purchase analysis 
#sort values to obtain top 5 
popular_items.sort_values("Purchase Count", ascending=False).head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
#convert notebook to markdown 
jupyter nbconvert --to markdown notebook.ipynb

```


      File "<ipython-input-32-99259d0986d2>", line 2
        jupyter nbconvert --to markdown notebook.ipynb
                        ^
    SyntaxError: invalid syntax
    


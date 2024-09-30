# SQL
Example of using SQL queries 

# 📊 Data Analytics and Testing Queries

**Welcome!** 👋 This repository showcases key SQL queries I regularly use in my workflow. They serve as templates for querying data related to user activity, ad monetization, AB-testing, and more. 

> **Disclaimer**: The data used in these queries does not represent any actual financial or personal data.

---

## ⚡ Quick Links

- [All User Activity by ID](#-all-user-activity-by-id)
- [Count Users by AB-Group](#-count-users-by-ab-group)
- [User Activity by Ads](#-user-activity-by-ads)
- [Users ANR by Ads](#-users-anr-by-ads)
- [Users Activity by Ad Network](#-users-activity-by-ad-network)
- [Count Networks Activity](#-count-networks-activity)

---

## 📚 Detailed Overview

### 📌 All User Activity by ID:

This query retrieves **all analytical events** of a specific user, showing activities like app interactions, event names, and AB-group membership.

```sql
SELECT 	created_at,
        activity_kind,
	event_name,
	JSONExtractString(base_parameters, 'ab_group') AS ab_group,
        base_parameters
FROM AdjustData.RealTimeAnalytics1
WHERE user_id = 'IDFA or GAID'
AND toDate(created_at) = today()
ORDER BY created_at DESC
```

## 📚 Count Users by AB-Group

### 📌 This query provides an overview of the distribution of unique users in different AB-groups:

```sql

SELECT	app_version,
	ab_group,
count (distinct user_id) AS users_unique -- user_id_example
FROM ProductData.ProjectName_product -- database_and_tableview_example
WHERE  ab_group LIKE '%01_001%' -- ab_group_number_example
GROUP BY app_version, ab_group
ORDER BY app_version, ab_group
```
## 📚 User Activity by Ads

### 📌 This includes analyzing user interactions with ads. The following SQL query retrieves the activity details of a specific user regarding ad views:

```sql

SELECT	created_at,
	JSONExtractString(base_parameters,'type') AS ad_type,
	JSONExtractString(base_parameters,'ad_placement') AS ad_placement,
	JSONExtractString(base_parameters,'status') AS ad_status,
	base_parameters -- global_events_example; used in json
FROM AdjustData.RealTimeAnalytics -- database_and_tableview_example
WHERE toDate(created_at) = today()
AND event_name = 'AdView'
AND user_id = 'IDFA or GAID' -- user_id_example
ORDER BY created_at DESC

```
## 📚 Users ANR by ads

### 📌 

```sql

SELECT	JSONExtractString(base_parameters,'status') AS ad_status,
	COUNT(ad_status) AS count
FROM AdjustData.RealTimeAnalytics
WHERE toDate(created_at) >= '2024-02-02'
AND event_name = 'AdView'
AND ad_status IN ('Fail', 'Complete', 'Start')
AND app_name = 'ProjectName'
GROUP BY ad_status
ORDER BY count DESC

```
## 📚 Users activity by ad network used

### 📌 

```sql
SELECT	app_name,
	created_at,
	store,
	ad_network,
	ad_placement,
	ad_type
FROM AdjustData.TableViewExample -- database_and_tableview_example
WHERE app_name = 'com.CompanyName.ProjectName'
AND toDate(created_at) = today()
AND activity_kind = 'ad_revenue'
AND ad_network = 'Google AdMob' -- for example
--AND ad_revenue_network IN ('Google AdMob', 'Pangle', 'Google Ad Manager')
AND user_id = '7952a7a1-26c1-48c9-993e-74b75b4f24a9' -- user_id_example
ORDER BY created_at DESC
```

## 📚 Count networks activity

### 📌

```sql
SELECT	ad_network AS networks,
	--uniqExact(ad_network),
	COUNT(ad_network) AS impressions
FROM AdjustData.TableViewExample -- database_and_tableview_example
WHERE app_name = 'com.CompanyName.ProjectName'
AND toDate(created_at) >= '2024-02-15'
AND activity_kind = 'ad_revenue'
AND networks <> ''
--AND os_name = 'android'
GROUP BY ad_revenue_network
ORDER BY impressions DESC

```


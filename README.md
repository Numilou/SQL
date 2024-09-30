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
- [Users States by Social ID](#-users-states-by-social-id)
- [Users States by Any Step](#-users-states-by-any-step)

---

## 📚 Detailed Overview

### 📌 All User Activity by ID

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


```sql

SELECT	app_version,
	ab_group,
count (distinct user_id) AS users_unique -- user_id_example
FROM ProductData.ProjectName_product -- database_and_tableview_example
WHERE  ab_group LIKE '%021%' -- ab_group_number_example
GROUP BY app_version, ab_group
ORDER BY app_version, ab_group

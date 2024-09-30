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
FROM AdjustData.RealTimeAnalytics
WHERE user_id = 'EF8C20B5-6CBD-4EF3-A3A5-ADDFCA1DF335'
AND toDate(created_at) = today()
ORDER BY created_at DESC

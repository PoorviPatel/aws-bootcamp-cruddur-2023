# Week 0 — Billing and Architecture

Frontend - JS using react
Backend - Python using flask
        - API = ORM ( Object Relational Mapping)

Requirements for Good Architecture: - 
- Verifible 
- Monitorable
- Traceable
- Feasible
Ex - Meets ISO Standards
   - 99.9% uptime
   - User can do specific tasks
   

The C4 model for visualizing Software Architecture
Context 
Containers
Components
Code


**Pricing Basics and Free Tier**

AWS Bill Free tier usage
-Free tier utilization and its better to check regularly that you are not using beyond free tier
-Change billing preferences to reeive invoice by email, receive free tier usage alarts
 and receive Billing alerts

Billing Alert(CloudWatch Alarm & Budget)
-To Create Alarm Select
	Create Alarm - > Select Metric - > Billing - > Total Estimate Charge - > USD - > Select Metric - > Setup you budget
- Setup cloudwatch billing alert
create new alert - 10 alarms are free in free tier
-Setup Budget using New Budget feature
-There are only 2 free budgets in free tier

Cost allocation tags
-Create and Manage Instances and cost allocated with one project using EC2 management console


Cost explorer
It helps to check cost and usage report using date range and other different filters.
Helps to generate the reports when needed

Calculate AWS estimates cost for service 

AWS credit
Reedem AWS credit

Free forever vs free for 12 months
Explor free for 12 months and always free options for free and the conditions in the tier type





	


**Crudder Conceptual Diagram**
https://lucid.app/lucidchart/53c1cf25-169e-47ce-bdd2-ab432786e02f/edit?viewport_loc=-146%2C64%2C2220%2C974%2C0_0&invitationId=inv_aef3bde5-3d03-48da-872a-6afadd78656f

**Crudder Logical diagram**
https://lucid.app/lucidchart/eef66c76-5f96-4589-be9f-4e8d265a954f/edit?viewport_loc=-1692%2C-462%2C2220%2C974%2C0_0&invitationId=inv_836ecba8-5036-4dc2-afe1-3294eff17186


# CampProject_pbix

Project Description
This project is a Power BI report based on data from several tables. The main goal is to visualize sales data, analyze the company's KPIs.

The project includes:

Data schematic model: illustrates the relationship between tables.
Tasks: 9 key tasks that will help define the current aspect of data analysis.

## Semantic model
**TSXR – Client Data.xlsm**
- Purpose: Master file for storing customer data and their spend.
- Contents:
  - **Client Top Level**: General information about customers (total spend, company size, region, etc.).
  - **Tech Spend (sections 1-3)**: Technology spend broken down by category (e.g. hardware, software, services).
  - **Personnel Spend (sections 1-3)**: Personnel spend (e.g. salaries, hiring, training).

**Key benchmarks.xlsx**
- Purpose: Store key benchmarks for comparing customer data.
- Contents:
  - **Key benchmarks**: Key metrics used to evaluate performance or spend across categories.
  
**Taxonomy Data.xlsx**
- Purpose: Reference tables for classifying data.
- Contents:
  - **SA Industries**: General industry categories for clients.
  - **Industry Mappings**: Relationships between client categories and benchmarks.
  - **SA Spend Codes**: Spend code table for classifying data in other tables.
  - **SA to Gartner Mappings**: Maps spend codes to Gartner categories.
  - **SA to CE Mappings**: Maps spend codes to Computer Economics categories.

The key task is to create a star schema semantic model. Try to use convenient names for the fact and dimension tables (F_tableName, D_tableName).

Use parameter to define and filter year of the benchmarks.

``` M 
    #"Filtered Rows1"= Table.SelectRows(#"Removed Columns", each [Year]= #"Benchmarks Year (2018)"),
    #"Merged Queries" = Table.NestedJoin(#"Filtered Rows1", {"Provider Industry Code"}, D_Industry_Mapping, {"Provider Industry Code"}, "D_Industry_Mapping", JoinKind.RightOuter),
```

Modify spend tables:

· Remove “AP Spend Code” column;

· Remove all unnecessary text from the “AP Spend Code Level 2” column, leave only the code itself (e.g., “AP086”)

## Pages
1. **G&A Spends**

Page Objective:

Provide an overview of General and Administrative (G&A) expenses, categorized by spend type, vendor, and spend nature (OPEX/CAPEX).
   - The page should have spend headlines (Total spend, OPEX spend, CAPEX spend, personnel, non personnel, total revenue).

   - Chart with spends group by Vendor name(It can be column chart).

   - Chart with spends group by AP first level category.

   - Table that represents top 10 spend items (Category, Vendor, Description, Spend amount).

   - In addition, the page should have an OPEX/CAPEX switch that affects all elements except spend headlines. To implement this use bookmarks or field parameter table.
  
2. **G&A spend contribution**
   
Page Objective:

Show how expenses are distributed across categories, highlighting contributions that account for 60% of total spends.
  
   - This page should display spends grouped by Spend Contribution, including:

   - Two visuals:

   - A stacked bar chart with AP Spend Category on the x-axis and Spend Contribution as the stacked values;

   - A table listing contributions that account for 60% of the total spends;

   - Two slicers: 
     - Business Unit; 
     - Country

3. **G&A spend by Business Unit**

Page Objective:

Analyze expenses categorized by Business Units.

   - Create a stacked chart similar to the previous page, with the stacked legend representing Business Units.

   - Include a table displaying the top 10 G&A Spend items with the following columns:

     - Spend Category;

     - Business Unit;

     - Spend Contribution;

     - Description;

     - OPEX/CAPEX;

     - Spend Amount;

4. **G&A Spend by Country**

Page Objective:

Show spend distribution across countries.

   - This page should include:
     - Spend Contribution Slicer
     - Business Unit Slicer
   - A bar chart showing the total G&A spend by AP spend category
   - A pie chart representing the total  G&A spend by country, with each segment showing the spend amount in millions or thousands. Users can filter the column chart by selecting a country on the pie chart.
5. **Non-G&A Spend**
   
Page Objective:

Compare G&A and Non-G&A expenses.

   - This should be the equivalent of G&A spends page, but with a column chart stacked by G&A and Non-G&A.
   - The spend headlines table should display G&A, Non-G&A, and Total spend.
   - The chart grouped by vendor should show values only for the top 10 Non-G&A spend items.
6. **Spend vs Gartner**

Page Objective:

Compare client spend with Gartner benchmarks for Capex, Opex, and Total IT spending.

   - You need to create a "Spend vs. Gartner" page. This page should feature two clustered column charts. The first chart will compare our client's Capex and Opex costs with those determined by Gartner. The second chart will display the costs divided by Gartner L1, which is why we have relationship to SubCategory in the benchmarks.

   - On this page, users should be able to switch between viewing costs and a percentage of revenue. Additionally, there should be an option to toggle between a "Side by Side" comparison and a "Difference" view. In the "Difference" view, the charts will display the client's costs minus Gartner's costs (no percentage of revenue difference is needed).

   - We will use benchmark data for 2018 and focus on the SA industry, which is set to 12. These custom generalized industries correspond to several Gartner industries, so we need to calculate and use the average value among them.
7. **Spend vs CE**


Page Objective:

Compare client expenses with Computer Electronics benchmarks.
   - The next page will be identical to the previous one, but now we are comparing with Computer electronics. For this provider, we have different types of benchmarks (Single value, Median, 25th/75th percentile). We will still have a clustered column chart, each cluster consisting of three columns (Client, Median, 25th percentile). Additionally, in addition to Opex and Capex, you should add Total (Benchmark name: Total IT spending as a Percentage of Revenue). These benchmarks do not specify Opex and Capex in the SubCategory, so you need to look at the benchmark name itself. The second graph looks the same as on the Gartner page, but ensure that the benchmark category is IT spend (not IT Staffing).
8. **Personnel vs CE**

Page Objective:

Compare client personnel spending with Computer Electronics benchmarks.
   - On this page, we should see the number of our employees compared to the benchmarks according to the AP spend category. User should have ability to switch betwean Side by Side and Difference. Another graph, where we can compare total personnel G&A spend vs CE (Personnel as % of IT ("Tech") Operational Spending), should contain columns for Client, 25th, 75th, and Median. Additionally, a table showing the number of employees, total spend, and average spend per employee, broken down by AP spend category and Client role.
9.  **Spend vs GT(Period)**

Page Objective:

Compare current client spending with post-reduction spending and CE benchmarks.

- We need to compare three indicators of tech spending: our current costs, costs after all reductions, and the CE benchmark. Include a toggle button for switching between Peer Median / 25th Percentile and Spend / Savings.

 - Additionally, there should be a stacked column chart displaying four columns: current costs, costs after the first, second, and third periods of reductions, stacked by AP spend category. For savings, we compare the projected(after reduction) spend minus current spend, CE peer median minus current spend, and CE 25th percentile minus current spend. The stacked column chart will show our savings for different periods, also grouped by AP spend category.

- A static table should display spend and savings for different periods (current, period 1/2/3) for various suppliers.
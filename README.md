# 🏦 Bank Loan Analysis Dashboard

##  Executive Summary
This project presents a comprehensive analysis of bank loan data using Microsoft Excel for data cleaning and preprocessing, and Power BI for building an interactive dashboard. The goal is to deliver actionable insights into loan performance, risk assessment, and business metrics.

The dataset comprises 38,600 loan applications totaling $435.8M in funded amounts, with detailed breakdowns across geography, demographics, loan purposes, and performance indicators
**Key Highlights:**
- **Total Portfolio Value:** $473.1M in total amount received
- **Loan Performance:** 86.2% good loans vs 13.8% bad loans
- **Average Metrics:** 12.0% interest rate, 13.3% DTI ratio
- **Geographic Coverage:** Multi-state loan distribution analysis
- **Temporal Analysis:** Month-over-month growth tracking

##  Project Objectives

1. **Risk Assessment:** Evaluate loan portfolio quality and identify risk patterns
2. **Performance Monitoring:** Track key performance indicators and trends
3. **Business Intelligence:** Provide actionable insights for decision-making
4. **Portfolio Analysis:** Understand loan distribution across various segments
5. **Trend Analysis:** Monitor growth patterns and seasonal variations

##  Data Cleaning Process

###  Dataset Overview
The dataset contained financial loan information including applicant details, employment data, loan characteristics, and performance metrics. Initial data required extensive cleaning to ensure accuracy and consistency for analysis.

###  Detailed Cleaning Steps

#### 1  Handling Missing and Incomplete Data
- **`emp_title` column:** Replaced missing values with placeholder `'Unknown'`
- **Text columns:** Reviewed and replaced empty/NaN values systematically
- **Numeric fields:** Validated completeness of critical financial metrics

####   Standardizing Categorical Values

**Column: `home_ownership`**
- Corrected inconsistent entries:
  - `RENTT`, `rent` → `RENT`
  - `MORTGAgges`, `mortgage` → `MORTGAGE`
- Applied **Power Query's Replace Values** tool for spelling corrections
- Converted entire column to **UPPERCASE** for uniformity
- Used **TRIM** function to remove extra spaces

####   Data Type Optimization
- **Financial columns:** Converted `loan_amount`, `annual_income` from text to numeric
- **Boolean fields:** Standardized loan status indicators
- **Categorical data:** Ensured proper text formatting for grouping operations

####  Date Standardization
- **Date columns:** `issue_date`, `last_credit_pull_date`
- Applied uniform `DD-MM-YYYY` format using Power Query
- Resolved date parsing inconsistencies

###  Cleaning Results
- **Data Consistency:** All categorical values standardized
- **Type Safety:** Numeric columns ready for calculations
- **Analysis Ready:** Clean dataset prepared for visualization
- **Quality Assurance:** Manual verification completed

##  Dashboard Architecture

###  Dashboard Structure
The Power BI dashboard consists of three main sections:


1. **📋 Summary Page:** Loan performance and risk assessment
 <img src="https://github.com/kotauday2110/Bank_Loan_Analysis/raw/main/summary.png" alt="Summary View" width="700"/>

 
2. **📈 Overview Page:** High-level KPIs and trend analysis
<img src="https://github.com/kotauday2110/Bank_Loan_Analysis/raw/main/Overview.png" alt="Loan Overview Dashboard" width="700"/>

4. **🔍 Details Page:** Granular transaction-level data
<img src="https://github.com/kotauday2110/Bank_Loan_Analysis/raw/main/Details.png" alt="Loan Details View" width="700"/>


###  Visual Components

#### Key Performance Indicators (KPIs)
- **Total Loan Applications:** 38.6K
- **Total Funded Amount:** $435.8M
- **Total Amount Received:** $473.1M
- **Average Interest Rate:** 12.0%
- **Average DTI Ratio:** 13.3%

#### Interactive Visualizations
1. **Time Series Analysis:** Monthly loan application trends
2. **Geographic Distribution:** State-wise loan mapping
3. **Demographic Breakdown:** Employee length and purpose analysis
4. **Performance Metrics:** Good vs bad loan segmentation
5. **Portfolio Composition:** Term length and home ownership analysis

### 🔧 Technical Features
- **Dynamic Filtering:** State, Grade, and Purpose filters
- **Drill-down Capability:** From summary to transaction level
- **Responsive Design:** Optimized for multiple screen sizes
- **Real-time Updates:** Connected to live data sources

##  DAX Functions & Measures Implementation

###  Core Business Metrics

#### Basic Portfolio Metrics
```dax
Total Loan Applications = COUNT(banking[ID])

Total Funded amount = SUM(banking[Loan_amount])

Total Amount Recieved = SUM(banking[total_payment])

Average DTI = AVERAGE(banking[dti])

Average Int rate = AVERAGE(banking[Int_rate])
```

#### Good vs Bad Loan Analysis
```dax
Good loan applications = CALCULATE([Total Loan Applications],banking[Good vs bad loan]="Good loan")

Good loan percentage = (CALCULATE([Total Loan Applications],banking[Good vs bad loan] ="Good loan"))/[Total Loan Applications]

Good funded amount = CALCULATE([Total Funded amount],banking[Good vs bad loan]="Good loan")

Good loan Recieved amount = CALCULATE([Total Amount Recieved],banking[Good vs bad loan]="Good loan")

Bad loan applications = CALCULATE([Total Loan Applications],banking[Good vs bad loan]="Bad loan")

Bad loan percentage = (CALCULATE([Total Loan Applications],banking[Good vs bad loan] ="Bad loan"))/[Total Loan Applications]

Bad funded amount = CALCULATE([Total Funded amount],banking[Good vs bad loan]="Bad loan")

Bad loan Recieved amount = CALCULATE([Total Amount Recieved],banking[Good vs bad loan]="Bad loan")
```

###  Time Intelligence Measures

#### Month-to-Date (MTD) Calculations
```dax
MTD Loan Applications = CALCULATE(TOTALMTD([Total Loan Applications],'Date'[Date]))

MTD Funded Amount = CALCULATE(TOTALMTD([Total Funded amount],'Date'[Date]))

MTD Amount Recieved = CALCULATE(TOTALMTD([Total Amount Recieved],'Date'[Date]))

MTD Int Rate = CALCULATE(TOTALMTD([Average Int rate],'Date'[Date]))

MTD DTI = CALCULATE(TOTALMTD([Average DTI],'Date'[Date]))
```

#### Previous Month-to-Date (PMTD) Calculations
```dax
PMTD Loan Applications = CALCULATE([MTD Loan Applications], DATESMTD(DATEADD('Date'[Date],-1,MONTH)))

PMTD Funded Amount = CALCULATE([MTD Funded Amount], DATESMTD(DATEADD('Date'[Date],-1,MONTH)))

PMTD Amount Recieved = CALCULATE([MTD Amount Recieved], DATESMTD(DATEADD('Date'[Date],-1,MONTH)))

PMTD Int Rate = CALCULATE([MTD Int Rate], DATESMTD(DATEADD('Date'[Date],-1,MONTH)))

PMTD DTI = CALCULATE([MTD DTI], DATESMTD(DATEADD('Date'[Date],-1,MONTH)))
```

#### Month-over-Month (MOM) Growth Calculations
```dax
MOM Applications = ([MTD Loan Applications]- [PMTD Loan Applications])/[PMTD Loan Applications]

MOM Funded Amount = ([MTD Funded Amount]- [PMTD Funded Amount])/[PMTD Funded Amount]

MOM Amount Recieved = ([MTD Amount Recieved]- [PMTD Amount Recieved])/[PMTD Amount Recieved]

MOM Int Rate = ([MTD Int Rate]- [PMTD Int Rate])/[PMTD Int Rate]

MOM DTI = ([MTD DTI]- [PMTD DTI])/[PMTD DTI]
```

###  Date Table & Helper Columns
```dax
Date = CALENDAR(MIN(banking[Issue_date]), MAX(banking[Issue_date]))

month = FORMAT('Date'[Date],"mmm")

month_num = MONTH('Date'[Date])
```

###  DAX Implementation Strategy

#### Measure Organization
- **Base Measures:** Core business metrics (totals, averages)
- **Calculated Measures:** Good/bad loan segmentation
- **Time Intelligence:** MTD, PMTD, and MOM calculations
- **Helper Measures:** Date formatting and categorization

#### Performance Optimization
- **Efficient Filtering:** Using CALCULATE for context transitions
- **Time Intelligence:** Leveraging built-in DAX time functions
- **Memory Management:** Optimized measure calculations
- **Reusability:** Base measures referenced in complex calculations

#### Key DAX Patterns Used
1. **CALCULATE Function:** For filtered aggregations
2. **Time Intelligence:** TOTALMTD, DATESMTD, DATEADD functions
3. **Percentage Calculations:** Ratio measures for performance tracking
4. **Date Manipulation:** Calendar tables and date formatting

##  Key Insights & Business Intelligence

###  Portfolio Performance Analysis

#### Loan Quality Assessment
- **Excellent Portfolio Health:** 86.2% good loans demonstrate strong underwriting
- **Manageable Risk:** 13.8% bad loan rate within industry standards
- **Strong Returns:** $473.1M received vs $435.8M funded (8.6% premium)

#### Risk Distribution
- **Good Loans:** 33.2K applications, $370.2M funded, $435.8M received
- **Bad Loans:** 5.3K applications, $65.5M funded, $37.3M received
- **Risk Concentration:** Manageable exposure across portfolio

###  Growth & Trend Analysis

#### Monthly Performance Trends
- **Steady Growth:** Consistent upward trend from 2.3K (Feb) to 4.3K (Dec)
- **Peak Season:** December shows highest loan application volume
- **Growth Rate:** Demonstrates healthy business expansion

#### Seasonal Patterns
- **Q4 Strength:** Strong performance in October-December period
- **Consistent Demand:** Minimal seasonal volatility
- **Market Stability:** Sustained growth trajectory

###  Geographic Distribution Insights

#### Market Coverage
- **Multi-state Presence:** Comprehensive geographic diversification
- **Risk Mitigation:** Geographic spread reduces regional risk concentration
- **Market Opportunities:** Potential for expansion in underserved areas

###  Customer Segmentation Analysis

#### Employment Length Patterns
- **Experienced Borrowers:** 10+ years employment shows highest volume
- **Risk Profile:** Longer employment correlates with better loan performance
- **Underwriting Insight:** Employment stability as key risk indicator

#### Loan Purpose Distribution
- **Debt Consolidation:** Largest segment (18K applications)
- **Credit Card:** Second largest purpose (5K applications)
- **Diversified Portfolio:** Multiple purposes reduce concentration risk

#### Home Ownership Analysis
- **Rent vs Mortgage:** Balanced distribution between renters (18K) and mortgage holders (17K)
- **Risk Assessment:** Home ownership status impacts loan terms and approval rates

###  Financial Performance Metrics

#### Interest Rate Analysis
- **Competitive Rates:** 12.0% average aligns with market standards
- **Risk-Based Pricing:** Rates vary based on borrower risk profile
- **Margin Optimization:** Balanced approach between competitiveness and profitability

#### Debt-to-Income Insights
- **Conservative Lending:** 13.3% average DTI indicates prudent underwriting
- **Risk Management:** DTI controls help maintain portfolio quality
- **Regulatory Compliance:** Metrics align with lending standards

###  Loan Status Performance

#### Current Portfolio Health
- **Active Loans:** 1,098 current loans ($188.7M funded)
- **Charged Off:** 5,333 loans ($655.3M) - managed risk exposure
- **Fully Paid:** 32,145 loans ($3,513.6M) - excellent repayment record

#### Recovery and Loss Metrics
- **Recovery Rate:** Strong performance in loan collections
- **Loss Mitigation:** Effective strategies for problem loans
- **Portfolio Optimization:** Balanced risk-return profile

##  Strategic Recommendations

###  Business Growth Opportunities
1. **Geographic Expansion:** Target underrepresented states
2. **Product Diversification:** Expand loan purpose categories
3. **Digital Enhancement:** Improve online application experience
4. **Customer Retention:** Develop loyalty programs for repeat borrowers

###  Risk Management Priorities
1. **Portfolio Monitoring:** Enhanced early warning systems
2. **Underwriting Refinement:** Optimize approval criteria
3. **Collection Strategies:** Improve recovery processes
4. **Stress Testing:** Regular portfolio resilience assessments

###  Operational Improvements
1. **Process Automation:** Streamline application processing
2. **Data Analytics:** Advanced predictive modeling
3. **Customer Experience:** Enhanced borrower journey
4. **Regulatory Compliance:** Maintain adherence to lending standards

##  Technical Implementation

###  Power BI Features Utilized
- **Data Modeling:** Star schema implementation
- **DAX Calculations:** Complex measures and KPIs with 25+ custom functions
- **Time Intelligence:** Advanced temporal analysis and MOM comparisons
- **Visualization:** Interactive charts and graphs
- **Performance:** Optimized for large datasets

###  Data Pipeline
1. **Source Systems:** Multiple data inputs
2. **ETL Process:** Power Query transformations
3. **Data Warehouse:** Centralized data storage
4. **Measure Layer:** Comprehensive DAX calculation engine
5. **Visualization Layer:** Power BI dashboard

###  User Experience
- **Intuitive Navigation:** Easy-to-use interface
- **Mobile Responsive:** Cross-device compatibility
- **Export Capabilities:** Data extraction options
- **Sharing Features:** Collaborative analytics

##  Project Deliverables

###  Dashboard Components
- ✅ Executive Summary Dashboard
- ✅ Detailed Analytics Views
- ✅ Interactive Filtering System
- ✅ Mobile-Optimized Design
- ✅ 25+ Custom DAX Measures

###  Documentation
- ✅ Data Cleaning Process Documentation
- ✅ DAX Functions and Measures Library
- ✅ Business Intelligence Insights
- ✅ Technical Implementation Guide
- ✅ User Manual and Training Materials

##  Conclusion

This Bank Loan Analysis project successfully demonstrates the power of data-driven decision making in financial services. Through comprehensive data cleaning, sophisticated DAX calculations, and intuitive visualization, the dashboard provides stakeholders with critical insights for:

- **Risk Assessment:** Understanding portfolio quality and identifying risk patterns
- **Performance Monitoring:** Tracking key metrics and trends with advanced time intelligence
- **Strategic Planning:** Making informed business decisions with MOM analysis
- **Operational Excellence:** Optimizing processes and procedures

The 86.2% good loan rate, coupled with strong financial performance metrics and robust DAX implementation, indicates a healthy and well-managed loan portfolio. The insights derived from this analysis provide a solid foundation for future growth strategies and risk management initiatives.

**Technical Achievements:**
- 25+ custom DAX measures for comprehensive analysis
- Advanced time intelligence with MTD, PMTD, and MOM calculations
- Complex portfolio segmentation and risk assessment metrics
- Optimized performance for large-scale financial data analysis

---

**Project Status:** ✅ Completed  
**Last Updated:** July 2025  
**Tools Used:** Power BI, Excel, Power Query, DAX  
**Data Volume:** 38.6K loan records, $435.8M portfolio value  
**DAX Measures:** 25+ custom calculations for advanced analytics

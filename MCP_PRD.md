
Document:  MCP Server for Fund Admin product
Document Type: Product Requirements Document
Version: V1
Author: Rama
Stakeholders: Engineering, Business.
Approvers: Business Team

**Problem Statement**
Fund managers want to streamline the fragmented process of downloading, analyzing and preparing Quarter / Year-end fund performance reports to their investors. Also reduce the TAT to respond to investor queries.

**Context**
Fund managers regularly monitor and analyse the current state of the fund to keep themselves ready to answer any investor queries.
During this analysis, they not only assess the performance of the fund to identify leaders and laggards but also review their sectoral exposure, risk associated with the investments. To perform the analysis, they have to export Fund level data, Portfolio and their KPIs data, Investor Commitments and Contributions data, into excel from various systems. They spend couple of days to manually prepare the reports to summarize the current state of fund to their investors. 

By providing an MCP server that integrates the fund administration platform with Gen AI tools (like Claude, Gemini, ChatGpt), users can extract insights for any queries, like the ones listed below, through natural language questions.
1. “Which portfolio companies have less than 12 months cash runway?”
2. “What is the total capital distributed in Fund II this year?”
3. “Summarize the current state of company X based on the latest KPI submitted?”
4. "How much unfunded capital is still left to make follow-on investments?"

**User Stories** 
User_Story_1
As a fund manager or fund administrator, 
I need my Gen AI application (Gemini / Claude / Open AI) access the data in fund administration app,
so that I can retrieve insights from underlying data using natural language queries.
Acceptance Criteria
1. Define the necessary JSON structure that users can include in their Gen AI configurations to access our MCP server.
2. Define the necessary authentication process for Gen AI platforms to access the fund admin MCP server.

User_Story_2
As a fund manager or fund administrator, 
I need fund admin app to respond to my queries related to fund data,
so that I need not login to fund admin app to retrieve the data I wanted. 
Acceptance Criteria
1. The MCP server must expose tools that cover all the available fund-level attributes. (Fund_size, capital_invested, current_value, distributed_capital, IRR%, Multiple, partners_committed, investments_made, management_fee_collected, expenses_incurred, income_accrued)
2. Given the firm has multiple funds, whenever user wants to know the Overal AUM across funds, the application must respond with sum of current value of fund across the funds.
3. Given the firm has multiple funds, whenever user wants to know the companies in which multiple funds belonged to the firm invested then application must respond with list of company names.
4. Given every Fund has its own NAV, whenever user wants to know the NAV of a specific fund, then application must respond with corresponding value.
5. Above are some sample queries and the application must be able to answer any queries for which the data is available.

User_Story_3
As a fund manager or fund administrator, 
I need fund admin app to respond to my queries related to portfolio,
so that I can assess the portfolio performance. 
Acceptance Criteria
1. The MCP server must expose tools that cover all the available portfolio-level attributes. (company_name, capital_invested, current_value, proceeds, unrealized_gains, IRR%, Multiple, investment_dates, invested_through, security_class_of_investment, custom_fields, valuation_prices)
2. Given the fund can invest multiple times in a company, whenever user wants to know how the value of the company changed since initial investment, then application must respond with the latest share prices updated in the valuation section for the company (if present)
3. Given the application tracks the IRR% / Multiple for every company, whenever user want to know the top 5 companies with highest IRR% / Multiple, then application must respond with the names of the companies and their IRR% / Multiple values ordered by highest values.
4. Above are some sample queries and the application must be able to answer any queries for which the data is available.

User_Story_4
As a fund manager or fund administrator, 
I need fund admin app to respond to my queries related to KPI data,
so that I can use them in my conversation with the founders. 
Acceptance Criteria
1. The MCP server must expose tools that cover all the available KPI attributes. (company_name, reporting_period, KPI_name, KPI_value, expected_value, actual_value)
2. Given the KPIs can span across the quarters for a company, whenever user wants to know the value of a specific KPI of the company, then application must respond with whatever is the last reported number along with the frequency (i.e., month or quarter in which it is submitted)
3. Given the same KPI is tracked across different companies in the portfolio, whenever user asks to fetch the companies with highest or lowest value (eg runway in months), then application must respond with the list of company names and corresponding metric value.
4. Above are some sample queries and the application must be able to answer any queries for which the data is available.

User_Story_5
As a fund manager or fund administrator, 
I need fund admin app to respond to my queries related to Investor numbers,
so that I can respond quickly to any queries sent by the investors.
Acceptance Criteria
1. The MCP server must expose tools that cover all the available investor data. (partner_name, commitment_amount, distributed_amount, fee_contributed, expense_contributed, date_of_commitment, and others)
2. Given the investor can contribute across the funds in the firm, whenever user wants to know the list of funds to which the investor committed, then application must retrieve the names of the funds in which the partner commitment is present.
3. Given the investors can be at different stages of the onboarding process, whenever user wants to know the list of investors who are yet to complete onboarding, then applicaiton must respond with partner names whose commitment status is neither committed nor exited.
4. Above are some sample queries and the application must be able to answer any queries for which the data is available.

User_Story_6
As an LP,
I need to analyse my portfolio across investment firms in which I invested,
So that I can assess my portfolio concentration and diversification.
Acceptance Criteria
1. The MCP server must expose tools that cover the data specific to the authenticated partner. (partner_name, commitment_amount, distributed_amount, fee_contributed, expense_contributed, date_of_commitment, and shared_document_name, shared_date)
2. Given the investor can contribute across the funds in the firm, whenever user wants to know the list of funds to which the investor committed, then application must retrieve the names of the funds in which the partner commitment is present.
3. Given the fund manager sends the regular reports, whenever the partners want to know if the report is present in the data room or not, then application must be able to identify whether the requested document is present or not and respond to partners. Application can also specify the date on which the document is shared.
4. Above are some sample queries and the application must be able to answer any queries for which the data is available.

User_Story_7
As a platform provider,
I need every query made by users on the platform to be recorded,
So that I can analyse how accurately the AI solution is answering the end user queries and derive inputs to improve it.
Acceptance Criteria
1. Track every query and application response as-is, along with attributes like user_id, user_name, firm_name, query_date, user_feedback
2. The user_feedback column can have values like like, dislike, no_comment.
3. Track the overall cost_incurred by the firm on daily basis.
4. List of queries that ended up with no_response due to lack of data, no_response due to technical issues like outside the scope of role, restricted information.

User_Story_8
As a platform provider,
I need to limit the usage of the AI solution by the client to certain tokens per day,  
so that I can ensure usage of one specific client doesn't impact other clients.
Acceptance Criteria
1. Track every query and application response as-is, along with attributes like tokens_consumed, cost_incurred.
2. Whenever the tokens_consumed by the client crossed the defined limit, 
a. Indicate to user that their limit is completed for the day. 
b. Continue serving user queries without interruption.

Other Requirements 
1. Every query must be responded as per the role access.
2. Fund managers query must be confined to the investment firm.
3. LPs query can be across firms that they have access to.
4. Critical information like LP contacts, LP and Fund bank information  must not be accessible to Gen AI and user queries related to them must be responded as "Sorry, I do not have access to this information."
5. Whenever tools return the null data as response, Gen AI must respond with "Sorry, I do not have access to this information."
6. Whenever tools return an error, Gen AI must respond with "Sorry, unable to retrieve data right now. Please try later."

**Out of scope**
1. Uploading documents and asking application to answer queries using the data in the documents is currently not supported. 
2. Writing or updating the data to the platform through natural language is not supported.
3. Company contact doesn't need any access.

**Success Metrics**
1. Activation Rate - Number of users using the feature for first time / Total number of active users on the platform.  
2. Adoption Rate - Number of queries per user / Total number of queries made by users on monthly basis.
3. Response Time - Time taken by application to deliver response to user since submission. Always must be less than 10 seconds.
4. Query Success Rate - Number of queries that returned a response / Total Number of queries made by users.

**Release Plan**
V1 must include User stories 1,2,3,4,7 & 8.
V2 must include User stories 5 & 6.
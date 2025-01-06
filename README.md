Bank-Loan-Report_Power-BI
Used DAX
Date Table = CALENDAR(MIN('financial_loan(1)'[issue_date]),MAX('financial_loan(1)'[issue_date]))
Month = FORMAT('Date Table'[Date], "mmm")
Month Number = MONTH('Date Table'[Date])
Select Measure 2 = { ("Total Loan Applications", NAMEOF('financial_loan(1)'[Total Loan Applications]), 0), ("Total Amount Received", NAMEOF('financial_loan(1)'[Total Amount Received]), 1), ("Total Funded Amount", NAMEOF('financial_loan(1)'[Total Funded Amount]), 2) }
Avg DTI = AVERAGE('financial_loan(1)'[dti])
Avg Int Rate = AVERAGE('financial_loan(1)'[int_rate])
Bad Loan % = [Bad Loan Applications]/[Total Loan Applications]
Bad Loan Applications = CALCULATE([Total Loan Applications],'financial_loan(1)'[Good vs Bad Loan] = "Bad Loan")
Bad Loan Funded Amount = CALCULATE([Total Funded Amount],'financial_loan(1)'[Good vs Bad Loan] = "Bad Loan")
Bad Loan Total Received = CALCULATE([Total Amount Received],'financial_loan(1)'[Good vs Bad Loan] = "Bad Loan")
Good Loan % = (CALCULATE([Total Loan Applications], 'financial_loan(1)'[Good vs Bad Loan]="Good Loan")/COUNT('financial_loan(1)'[id]))
Good Loan Applications = CALCULATE([Total Loan Applications],'financial_loan(1)'[Good vs Bad Loan] = "Good Loan")
Good Loan Funded Amount = CALCULATE([Total Funded Amount],'financial_loan(1)'[Good vs Bad Loan] = "Good Loan")
Good Loan Total Received = CALCULATE([Total Amount Received],'financial_loan(1)'[Good vs Bad Loan] = "Good Loan")
LMTD Avg DTI = CALCULATE(AVERAGE('financial_loan(1)'[dti]), DATESMTD(DATEADD('Date Table'[Date].[Date],-1,MONTH)))
LMTD Avg Int Rate = CALCULATE(AVERAGE('financial_loan(1)'[int_rate]),DATESMTD(DATEADD('Date Table'[Date],-1,MONTH)))
LMTD Loan Applications = CALCULATE(COUNT('financial_loan(1)'[id]), DATESMTD(DATEADD('Date Table'[Date],-1,MONTH)))
LMTD Total Amont Received = CALCULATE([Total Amount Received],DATESMTD(DATEADD('Date Table'[Date],-1,MONTH)))
LMTD Total Funded Amont = CALCULATE([Total Funded Amount],DATESMTD(DATEADD('Date Table'[Date],-1,MONTH)))
MTD Amount Received = CALCULATE(TOTALMTD([Total Amount Received],'Date Table'[Date]))
MTD Applications = CALCULATE(TOTALMTD(COUNT('financial_loan(1)'[id]),'Date Table'[Date].[Date]))
MTD Avg DTI = CALCULATE(TOTALMTD(AVERAGE('financial_loan(1)'[dti]),'Date Table'[Date]))
MTD Avg Int Rate = CALCULATE(TOTALMTD(AVERAGE('financial_loan(1)'[int_rate]),'Date Table'[Date]))
MTD Funded Amount = CALCULATE(TOTALMTD([Total Funded Amount],'Date Table'[Date]))
Total Amount Received = SUM('financial_loan(1)'[total_payment])
Total Funded Amount = SUM('financial_loan(1)'[loan_amount])
Total Loan Applications = COUNT( 'financial_loan(1)'[id] )
YOY Amount Received = ([MTD Amount Received]-[LMTD Total Amont Received])/[LMTD Total Amont Received]
YOY Avg Int Rate = ([MTD Avg Int Rate]-[LMTD Avg Int Rate])/[LMTD Avg Int Rate]
YOY DTI = ([MTD Avg DTI] - [LMTD Avg DTI])/[LMTD Avg DTI]
YOY Loan Amount = ([MTD Funded Amount]-[LMTD Total Funded Amont])/[LMTD Total Funded Amont]
YOY Loan Applications = ([MTD Applications]-[LMTD Loan Applications])/[LMTD Loan Applications]

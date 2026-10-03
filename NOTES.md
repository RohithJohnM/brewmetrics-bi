# Copilot-Assisted DAX Development Notes

## Measure 1: Month-over-Month Sales Growth %

### Copilot's Initial Suggestion
Copilot suggested calculating current-month sales using SUM(Fact_Sales[sales_amount]) and previous-month sales using CALCULATE with PREVIOUSMONTH(Dim_Date[date]). It then calculated the percentage change using DIVIDE.

### Review / Correction
The suggested measure was directly usable. The Dim_Date table is related to Fact_Sales through the date column, and PREVIOUSMONTH correctly shifts the date context to the previous month. DIVIDE also safely handles cases where previous-month sales are blank or zero.

### Final Measure
Month-over-Month Sales Growth %
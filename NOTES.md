# Copilot-Assisted DAX Development Notes

## Measure 1: Month-over-Month Sales Growth %

### Copilot's Initial Suggestion
Copilot suggested calculating current-month sales using SUM(Fact_Sales[sales_amount]) and previous-month sales using CALCULATE with PREVIOUSMONTH(Dim_Date[date]). It then calculated the percentage change using DIVIDE.

### Review / Correction
The suggested measure was directly usable. The Dim_Date table is related to Fact_Sales through the date column, and PREVIOUSMONTH correctly shifts the date context to the previous month. DIVIDE also safely handles cases where previous-month sales are blank or zero.

### Final Measure
Month-over-Month Sales Growth %

---

## Measure 2: Cumulative Sales

### Copilot's Initial Suggestion
Copilot suggested a cumulative sales measure using MAX(Dim_Date[date]) to identify the current date and FILTER with ALL(Dim_Date) to include all dates up to the current date.

### Review / Correction
The initial suggestion used ALL(Dim_Date), which removes filters from the entire date dimension. I changed this to ALL(Dim_Date[date]) so that only the date-column filter is removed while other date-dimension filters can remain in the filter context.

### Final Measure
Cumulative Sales
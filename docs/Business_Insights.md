# Business Insights

This note explains the findings visible in the **E-Commerce Sales Analytics** Power BI report. Observations refer to the full period **1 December 2010–9 December 2011**, unless stated otherwise.

## 1. Sales activity and returns

- The report shows approximately **£10.67M in positive-line revenue**.
- Cancellation-type line amounts total approximately **−£896.81K**.
- The resulting **Net Revenue** is approximately **£9.77M**.
- A subset of descriptions classed as non-product contributes **£203.80K net**, yielding **£9.57M Net Product Revenue** under the current rule.

**Business implication:** positive sales alone overstate the revenue remaining after cancellation-type entries. Revenue reporting should explicitly separate gross positive-line sales, cancellations, and non-merchandise charges.

## 2. Geographic concentration

The **United Kingdom** contributes the overwhelming majority of revenue among the countries displayed, with the Netherlands, EIRE, Germany and France following in the Top 5.

**Follow-up:** compare customer counts and average order value by market before determining whether other markets represent growth opportunities.

## 3. Seasonality and monthly revenue

The monthly line chart rises toward a peak around **November 2011**.

**Interpretation warning:** the source period ends on **9 December 2011**, so the displayed December amount is not a complete calendar month and cannot fairly be compared as a full-month result.

## 4. Product ranking caveat

`PAPER CRAFT, LITTLE BIRDIE` and `MEDIUM CERAMIC TOP STORAGE JAR` are highly prominent in the **positive-line quantity** ranking. The original data also contains large cancellation transactions for these items.

**Follow-up:** develop a separate **net quantity** measure and investigate unusually large invoice lines before treating gross quantity as a demand forecast.

## 5. Customer segmentation

Top customer rankings differ depending on whether customers are ranked by **revenue** or by **number of orders**.

**Follow-up:** test segmentation by frequency and value, and consider recency only after defining stable observation windows and customer identifiers.

## Recommended future improvements

1. Expand and audit the non-product/fee classification, including additional service and adjustment descriptions.
2. Report gross and net quantities side by side for product performance.
3. Introduce a dedicated calendar table and shared dimensions if the model grows.
4. Add data-quality checks for missing customer IDs, unusual prices, duplicates and very large quantities.
5. Publish a reproducible definition sheet for each KPI.

These are suggested next steps, **not** claims that the current dashboard already contains these analyses.

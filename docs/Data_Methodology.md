# Data Methodology

## Source and period

The project uses the **UCI Online Retail** transaction dataset. The report period covers **1 December 2010 to 9 December 2011**, with partial coverage for the boundary months.

- **InvoiceNo:** invoice identifier; identifiers beginning with `C` are treated as cancellation-type transactions.
- **StockCode / Description:** item identifiers and item description.
- **Quantity:** positive and negative transaction quantities.
- **InvoiceDate:** timestamp for a transaction.
- **UnitPrice:** unit price.
- **CustomerID:** may be blank.
- **Country:** customer/transaction country provided by the dataset.

## Power Query analysis tables

### `Sales_Analysis` (positive sales)

Reference of the source data retaining:
- `TransactionType = "Other"` (rather than an invoice beginning with `C`);
- `Quantity > 0`;
- `UnitPrice > 0`.

For retained records:
```text
LineRevenue = Quantity × UnitPrice
```

The reported `Total Revenue`, `Total Orders`, `Average Order Value`, and `Total Customers` are based on this positive-sales scope. **Total Revenue should not be called net revenue.**

### `Sales_Net_Analysis` (net lines)

Reference retaining line items with **`UnitPrice > 0`** and including both positive and negative quantities.

```text
NetLineRevenue = Quantity × UnitPrice
```

The current reconciliation treats the invoices starting with `C` as cancellation lines and sums the signed line revenue across the table.

**Scope limitation:** adjustments with negative `UnitPrice` and zero-price rows are excluded; this is not a complete accounting general ledger.

## Current ProductType classification

Transactions are identified as `Non-product` when `Description` is null or its trimmed uppercase value matches one of:

- `DOTCOM POSTAGE`
- `POSTAGE`
- `MANUAL`

All other rows are categorized `Product`. This deliberately narrow rule should be reviewed before making financial claims: other non-product descriptions (e.g., fees) may exist.

### Full-period non-product reconciliation

| Description | Net amount |
| --- | ---: |
| DOTCOM POSTAGE | £206,245.48 |
| MANUAL | −£68,671.64 |
| POSTAGE | £66,230.64 |
| **Total** | **£203,804.48** |

## Measures and filtering

```dax
Total Revenue = SUM(Sales_Analysis[LineRevenue])

Total Orders = DISTINCTCOUNT(Sales_Analysis[InvoiceNo])

Average Order Value =
DIVIDE([Total Revenue], [Total Orders], 0)

Total Customers =
DISTINCTCOUNTNOBLANK(Sales_Analysis[CustomerID])
```

`Net Revenue` and `Net Product Revenue` use `Sales_Net_Analysis[NetLineRevenue]` with **TREATAS** mapping selected `Sales_Analysis[Country]` and `Sales_Analysis[OrderDate]` to the corresponding `Sales_Net_Analysis` fields. `Net Product Revenue` additionally applies `ProductType = "Product"`.

`OrderDate` is a date-only column derived from the invoice timestamp in both tables, so timestamp hours do not prevent daily matching.

### Validation check

```text
£10,666,684.54 - £896,812.49 = £9,769,872.05
£9,769,872.05 - £203,804.48 = £9,566,067.57
```

The report was tested with a country slicer and a date-range slicer. The net revenue measure was corrected so it changes when the date range is restricted.

## Known analytical caveats

1. **Gross versus net ranking:** unusually large positive quantities, such as `PAPER CRAFT, LITTLE BIRDIE` and `MEDIUM CERAMIC TOP STORAGE JAR`, can have corresponding cancellation lines. The Top 10 quantity chart shows positive-line quantity, not net sold units.
2. **Date boundaries:** the final month is incomplete. Avoid reading the December drop as a full-month performance decline.
3. **Product classification:** current category is rule-based and not exhaustive.
4. **Customer attribution:** blank customer identifiers are excluded from distinct customer counts.
5. **Comparability:** the net measures and positive-line measures come from separate tables. Keep slicer/measure mappings consistent if extending the model.

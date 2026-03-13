// =============================================
// AWESOME CHOCOLATES - DAX MEASURES
// =============================================

// ── Core Aggregations ──────────────────────────────────────────────────────

total sales =
    SUM(shipments[Sales])

total boxes =
    SUM(shipments[Boxes])

total shipments =
    COUNTROWS(shipments)

Total cost =
    SUM(shipments[Cost])

Total profit =
    [total sales] - [total boxes]

Profit % =
    DIVIDE([Total profit], [total sales])


// ── LBS (Low Box Shipments) ────────────────────────────────────────────────

LBS count =
    CALCULATE([total shipments], shipments[Boxes] < 50)

LBS % =
    DIVIDE([LBS count], [total shipments])

LBS down target = 0.1

LBS % indicator =
    IF(
        [LBS %] > [LBS down target], 2,
        IF([LBS %] > 0.05 * [LBS down target], 1, 0)
    )


// ── Profit Target ──────────────────────────────────────────────────────────

Profit target = 0.6

Profit target Indicator =
    IF([Profit %] > [Profit target], 2, 1)


// ── Latest Date Context ────────────────────────────────────────────────────

latest date =
    LASTDATE('calendar'[Start of Month])

total sales latest month =
    VAR ld = [latest date]
    RETURN
        CALCULATE([total sales], 'calendar'[Start of Month] = ld)

total boxes latest month =
    VAR ld = [latest date]
    RETURN
        CALCULATE([total boxes], 'calendar'[Start of Month] = ld)

total shipment latest month =
    VAR ld = [latest date]
    RETURN
        CALCULATE([total shipments], 'calendar'[Start of Month] = ld)

total cost latest month =
    VAR ld = [latest date]
    RETURN
        CALCULATE([Total cost], 'calendar'[Start of Month] = ld)

total profit latest month =
    VAR ld = [latest date]
    RETURN
        CALCULATE([Total profit], 'calendar'[Start of Month] = ld)


// ── Previous Month ─────────────────────────────────────────────────────────

total sales(prevs month) =
    CALCULATE([total sales], PREVIOUSMONTH('calendar'[Date]))

total boxes(prevs month) =
    CALCULATE([total boxes], PREVIOUSMONTH('calendar'[Date]))

total shipment(prevs month) =
    CALCULATE([total shipments], PREVIOUSMONTH('calendar'[Date]))

total cost(prevs month) =
    CALCULATE([Total cost], PREVIOUSMONTH('calendar'[Date]))

total profit(prevs month) =
    CALCULATE([Total profit], PREVIOUSMONTH('calendar'[Date]))


// ── Month-over-Month % Changes (Slicer-driven) ─────────────────────────────

MOM Sales change % =
    VAR this_month = [total sales]
    VAR prev_month = [total sales(prevs month)]
    RETURN
        DIVIDE(this_month - prev_month, prev_month)

MOM Boxes change % =
    VAR this_month = [total boxes]
    VAR prev_month = CALCULATE([total boxes], PREVIOUSMONTH('calendar'[Date]))
    RETURN
        DIVIDE(this_month - prev_month, prev_month)

MOM cost change % =
    VAR this_month = [Total cost]
    VAR prev_month = CALCULATE([Total cost], PREVIOUSMONTH('calendar'[Date]))
    RETURN
        DIVIDE(this_month - prev_month, prev_month)

MOM profit change % =
    VAR this_month = [Total profit]
    VAR prev_month = CALCULATE([Total profit], PREVIOUSMONTH('calendar'[Date]))
    RETURN
        DIVIDE(this_month - prev_month, prev_month)

MOM shipments change % =
    VAR this_month = [total shipments]
    VAR prev_month = CALCULATE([total shipments], PREVIOUSMONTH('calendar'[Date]))
    RETURN
        DIVIDE(this_month - prev_month, prev_month)


// ── Latest MOM % Changes (always shows most recent month delta) ────────────

latest mom sales change % =
    VAR ld = [latest date]
    VAR this_month_sales = [total sales latest month]
    VAR prev_month_sales = CALCULATE([total sales], 'calendar'[Start of Month] = EDATE(ld, -1))
    RETURN
        DIVIDE(this_month_sales - prev_month_sales, prev_month_sales)

latest mom boxes change % =
    VAR ld = [latest date]
    VAR this_month_boxes = [total boxes latest month]
    VAR prev_month_boxes = CALCULATE([total boxes], 'calendar'[Start of Month] = EDATE(ld, -1))
    RETURN
        DIVIDE(this_month_boxes - prev_month_boxes, prev_month_boxes)

latest mom shipment change % =
    VAR ld = [latest date]
    VAR this_month_shipment = [total shipment latest month]
    VAR prev_month_shipment = CALCULATE([total shipments], 'calendar'[Start of Month] = EDATE(ld, -1))
    RETURN
        DIVIDE(this_month_shipment - prev_month_shipment, prev_month_shipment)

latest mom cost change % =
    VAR ld = [latest date]
    VAR this_month_cost = [total cost latest month]
    VAR prev_month_cost = CALCULATE([Total cost], 'calendar'[Start of Month] = EDATE(ld, -1))
    RETURN
        DIVIDE(this_month_cost - prev_month_cost, prev_month_cost)

latest mom profit change % =
    VAR ld = [latest date]
    VAR this_month_profit = [total profit latest month]
    VAR prev_month_profit = CALCULATE([Total profit], 'calendar'[Start of Month] = EDATE(ld, -1))
    RETURN
        DIVIDE(this_month_profit - prev_month_profit, prev_month_profit)


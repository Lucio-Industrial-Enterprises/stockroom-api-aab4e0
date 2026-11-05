# Warehouse tools

Small Python utilities for warehouse order and shipping workflows.

Run the tests with `python3 -m unittest discover -s tests -v`.

## Shipping

`warehouse.rates.shipping_quote(weight_kg, zone, service="standard")` returns a
delivery quote in dollars, rounded to two decimal places. Zones are `local` and
`regional`; weights must be positive. Services are `standard` (the default) and
`express`. Unknown zones or services and non-positive weights raise `ValueError`.

Standard shipping costs $5 for local or $8 for regional delivery, plus $1.50 per
kilogram. Express adds $7 for either zone. Existing two-argument calls continue
to return standard quotes.

```python
from warehouse.rates import shipping_quote

shipping_quote(2, "local")                     # 8.0
shipping_quote(2, "local", service="express")  # 15.0
shipping_quote(2, "regional", "express")       # 18.0
```

The command-line entry point prints a quote for an order:

```sh
python3 -m warehouse.cli 2 local                     # $8.00
python3 -m warehouse.cli 2 regional --service standard # $11.00
python3 -m warehouse.cli 2 regional --service express  # $18.00
```

Omitting `--service` selects standard shipping; unsupported services are rejected.

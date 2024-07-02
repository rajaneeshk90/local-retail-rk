# Network and Domain Specific Rules in YAML

These rules can be tested by downloading the Postman collection [here] and using the validator available [here].

## 1. SEARCH

1. `Intent.tags[].descriptor.code` should be in `["BUYER_FINDER_FEES", "SETTLEMENT_TERMS"]`.
2. If `Intent.tags[].descriptor.code = BUYER_FINDER_FEES`, then `Intent.tags[].list[].descriptor.code` should be in `["BUYER_FINDER_FEES_PERCENTAGE", "BUYER_FINDER_FEES_AMOUNT"]`.
    - If `Intent.tags[].list[].descriptor.code` is `BUYER_FINDER_FEES_PERCENTAGE` then `Intent.tags[].list[].value` should be between `0 - 99`.
    - If `Intent.tags[].list[].descriptor.code` is `BUYER_FINDER_FEES_AMOUNT` then `Intent.tags[].list[].value` should be any amount.
3. If `Intent.tags[].descriptor.code = SETTLEMENT_TERMS`, then `Intent.tags[].list[].descriptor.code` should be in `["SETTLEMENT_WINDOW", "SETTLEMENT_BASIS", "MANDATORY_ARBITRATION", "COURT_JURISDICTION"]` etc.
    - If `Intent.tags[].list[].descriptor.code` is `SETTLEMENT_WINDOW` then `Intent.tags[].list[].value` should be a valid duration.
    - If `Intent.tags[].list[].descriptor.code` is `SETTLEMENT_BASIS` then `Intent.tags[].list[].value` should be `"INVOICE_RECEIPT"`.
    - If `Intent.tags[].list[].descriptor.code` is `MANDATORY_ARBITRATION` then `Intent.tags[].list[].value` should be either `"TRUE"` or `"FALSE"`.
    - If `Intent.tags[].list[].descriptor.code` is `COURT_JURISDICTION` then `Intent.tags[].list[].value` should be a valid string.
    - If `Intent.tags[].list[].descriptor.code` is `STATIC_TERMS` then `Intent.tags[].list[].value` should be a valid URI.
    - If `Intent.tags[].list[].descriptor.code` is `SETTLEMENT_TYPE` then `Intent.tags[].list[].value` should be `["neft", "upi", "rtgs"]`.
    - If `Intent.tags[].list[].descriptor.code` is `SETTLEMENT_AMOUNT` then `Intent.tags[].list[].value` should be an amount.
4. `Intent.fulfillment.type` should be in `["DELIVERY", "PICKUP"]`.
5. `Intent.fulfillment.stops[].location.gps` is required.

## 2. on_search

1. If `message.catalog.providers[].items[].category_ids` has `"c1"`, then `message.catalog.providers[].items[].tags[].list.descriptor.code` should be in `["date_of_manufacturing", "date_of_expiry", "country_of_origin", "type"]`, where all 4 are mandatory and none can be repeated.
2. If `message.catalog.providers[].items[].category_ids` has `"c2"`, then `message.catalog.providers[].items[].tags[].list.descriptor.code` should be in `["user-manual", "technical-specs", "model-no"]`, where all 3 are mandatory and none can be repeated.
3. `message.catalog.providers[].descriptor.name`, `short_desc`, `long_desc`, `images` are mandatory.
4. `message.catalog.providers[].categories[].descriptor.code` should be in `["grocery", "electronic"]`, where code is mandatory.
5. `message.catalog.providers[].categories[]` should have at least one item.
6. `message.catalog.providers[].fulfillments[].type` should be in `["DELIVERY", "PICKUP"]` where both id and type are mandatory.
7. `message.catalog.providers[]` should have `id`, `descriptor`, `categories`, `fulfillments`, `items`.
8. `message.catalog.providers[].items[]` should have `id`, `descriptor`, and `price`.

## 3. select

1. `message.order` should have `provider`, `items` object.
2. `message.order.provider` should have `id`.
3. `message.order.items[]` should have at least one object.
4. `message.order.items[]` should have `id` and `quantity`.
5. `message.order.items[].quantity` should have `selected`.
6. `message.order.items[].quantity.selected` should have `count`.
7. If `message.order.fulfillments[].type = "DELIVERY"`, then `message.order.fulfillments[].stops[].location` is mandatory.
8. `message.order.fulfillments[].type` should be in `["DELIVERY", "PICKUP"]`.

## 4. init

1. `message.order` should have `provider`, `items` object.
2. `message.order.provider` should have `id`.
3. `message.order.items[]` should have at least one object.
4. `message.order.items[]` should have `id` and `quantity`.
5. `message.order.items[].quantity` should have `selected`.
6. `message.order.items[].quantity.selected` should have `count`.
7. `message.order.fulfillments[].type` should be in `["DELIVERY", "PICKUP"]`, and type is required.
8. If `message.order.fulfillments[].type = "DELIVERY"`, then `message.order.fulfillments[].stops[].location` is mandatory.

## 5. on_init

1. `message.order` should have `provider`, `items`, `fulfillments`, `quote`, `billing`, `payments`, `cancellation_terms`.
2. `message.order.items[]` should have at least one object.
3. Either `percentage` or `amount` is allowed in `message.order.cancellation_terms`, not both.

## 6. confirm

1. If `order.payments[].type = "PRE-ORDER"`, then `order.payments[].params.transaction_id` is required.

## 7. on_confirm

1. `order.fulfillments[].state.descriptor.code` should be `ACCEPTED`.
2. `order.fulfillments[].state` should be required.

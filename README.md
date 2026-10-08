# Tech Challenge — E-commerce API

Build a small e-commerce REST API using **Python**, **FastAPI**, and
**PostgreSQL**. The goal is a clean, easy-to-understand foundation rather
than a production-complete system.

## Scope

Implement three entities:

- **Product** — includes a `quantity` field representing available stock.
- **Cart** — can contain one or more products.
- **Payment** — records a payment request for a cart.

Keep the model intentionally simple. A cart item should associate a product
with its requested quantity; avoid discounts, shipping, authentication, order
fulfilment, or other extra business rules for now.

### Good-to-have fields

The exact schema is up to you, but these fields provide a useful starting
point:

- **Product:** `id`, `name`, `description`, `price`, `quantity`, `created_at`,
  and `updated_at`.
- **Cart:** `id`, `status`, `items`, `created_at`, and `updated_at`.
- **Cart item:** `product_id`, `quantity`, and `unit_price`. The cart total can
  be calculated from its items rather than stored.
- **Payment:** `id`, `cart_id`, `amount`, `status`, `card_last_four`, and
  `created_at`.

Use an appropriate decimal type for monetary values. Useful cart statuses
include `open`, `paid`, and `cancelled`; payment statuses can be as simple as
`pending`, `approved`, and `declined`.

## Required API routes

Expose clear REST endpoints under `/products`, `/carts`, and `/payments`.

### Products

- `POST /products` — create a product.
- `GET /products` — list products.
- `GET /products/{product_id}` — retrieve one product.
- `PUT /products/{product_id}` — update a product, including `quantity`.
- `DELETE /products/{product_id}` — delete a product.

### Carts

- `POST /carts` — create a cart.
- `GET /carts` — list carts.
- `GET /carts/{cart_id}` — retrieve a cart and its items.
- `PUT /carts/{cart_id}` — update a cart and its products.
- `DELETE /carts/{cart_id}` — delete a cart.

### Payments

- `POST /payments` — submit a cart for payment along with credit-card
  information.

The payment endpoint may simulate approval; no payment gateway integration is
required. The request body must identify the cart and provide the card
details. For example:

```json
{
  "cart_id": "8f3cc6f8-b9bc-47e1-9b5a-d97a458b6f08",
  "card": {
    "holder_name": "Ada Lovelace",
    "number": "4111111111111111",
    "expiry_month": 12,
    "expiry_year": 2030,
    "cvv": "123"
  }
}
```

The created payment should reference the cart, capture its amount, and return
a simple result such as `approved` or `declined`. Never persist the full card
number or CVV; storing only masked data such as `card_last_four` is enough for
this challenge.

As an optional improvement, consider accepting an `idempotency_key` in the
request body to make retries safe. This is a suggestion and is not required to
complete the challenge.

## Technical requirements

- Python 3.11+.
- FastAPI for the HTTP API and automatic OpenAPI documentation.
- PostgreSQL for persistence.
- Pydantic models for request validation and response schemas.
- An ORM or SQL layer of your choice (for example, SQLAlchemy).
- Configuration through environment variables; do not commit credentials.

## Definition of done

- The API starts locally and connects to PostgreSQL.
- All required routes are implemented and documented in `/docs`.
- Products and carts support the requested CRUD operations.
- A cart can contain multiple products with quantities.
- Payment accepts a cart and credit-card details without storing sensitive
  card data.
- Basic tests cover the main happy paths and validation errors.

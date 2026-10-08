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

## Required API routes

Expose clear REST endpoints under `/products` and `/carts`.

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
required. Never persist raw credit-card data. Return a simple payment result
such as `approved` or `declined`.

## Technical requirements

- Python 3.11+.
- FastAPI for the HTTP API and automatic OpenAPI documentation.
- PostgreSQL for persistence.
- Pydantic models for request validation and response schemas.
- An ORM or SQL layer of your choice (for example, SQLAlchemy).
- Configuration through environment variables; do not commit credentials.

## Suggested project structure

```text
app/
  main.py          # FastAPI application and route registration
  database.py      # PostgreSQL connection/session setup
  models/          # persistence models
  schemas/         # Pydantic request and response models
  routers/         # product, cart, and payment routes
  services/        # small business-rule helpers, if needed
tests/
```

## Definition of done

- The API starts locally and connects to PostgreSQL.
- All required routes are implemented and documented in `/docs`.
- Products and carts support the requested CRUD operations.
- A cart can contain multiple products with quantities.
- Payment accepts a cart and credit-card details without storing sensitive
  card data.
- Basic tests cover the main happy paths and validation errors.

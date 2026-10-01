# DummyJSON Products API

The DummyJSON Products API provides sample e-commerce product data for learning and practicing how RESTful APIs work. This document could be used by software developers or testers. The base URL here is `https://dummyjson.com/products` and it doesn't need any specific setup before being used.

**Note:** This is a practice API. `POST`, `PUT`, and `DELETE` requests all return realistic-looking responses, but none of them actually persist — a fresh `GET` request afterward will always show the original, unmodified data.

The following endpoints are covered in this document.

- `GET /products` — get a list of all the products
- `GET /products/{id}` — find the product with the mentioned id
- `GET /products/search` — search products by keyword
- `POST /products/add` — add a new product
- `PUT /products/{id}` — edit an existing product
- `DELETE /products/{id}` — delete a product

## List Products

**Method:** `GET`
**Path:** `/products`

Returns a paginated list of all products in the catalog. This endpoint takes no parameters — every call returns the same underlying data, just split into pages.

Each product object includes fields such as `id`, `title`, `description`, `category`, `price`, `brand`, `tags`, and `reviews`. Several additional fields (`returnPolicy`, `minimumOrderQuantity`, `dimensions`, `meta`, and others) exist on every product but are omitted from the example below for brevity.

**Response Fields:**

| Field | Description |
|---|---|
| `products` | Array of product objects for this page of results |
| `total` | Total number of products in the entire catalog |
| `skip` | Number of products skipped before this page begins |
| `limit` | Maximum number of products returned per request |

**Response Example:**

```json
{
  "products": [
    {
      "id": 101,
      "title": "Apple AirPods Max Silver",
      "tags": ["electronics", "over-ear headphones"],
      "reviews": [
        {
          "rating": 5,
          "comment": "Excellent quality!",
          "date": "2025-04-30T09:41:02.053Z",
          "reviewerName": "Henry Adams",
          "reviewerEmail": "henry.adams@x.dummyjson.com"
        }
      ]
    }
  ],
  "total": 100,
  "skip": 0,
  "limit": 30
}
```

**Possible Responses:**

| Status | Meaning |
|---|---|
| `200 OK` | The list of products is returned |

## Find Product by ID

**Method:** `GET`
**Path:** `/products/{id}`

**Path Parameters:**

| Parameter | Required | Description |
|---|---|---|
| `id` | Yes | The unique ID of the product to retrieve |

This endpoint returns the details of a single product matching the given ID. The product object includes fields such as `id`, `title`, `description`, `category`, `price`, `brand`, `tags`, and `reviews`. Several additional fields (`returnPolicy`, `minimumOrderQuantity`, `dimensions`, `meta`, and others) exist but are omitted from the example below for brevity.

**Response Example:**

```json
{
    "id": 188,
    "title": "Pampi Shoes",
    "tags": [
        "footwear",
        "casual shoes"
    ],
    "warrantyInformation": "No warranty",
    "shippingInformation": "Ships in 1 week",
    "availabilityStatus": "In Stock",
    "reviews": [
        {
            "rating": 4,
            "comment": "Highly recommended!",
            "date": "2025-04-30T09:41:02.054Z",
            "reviewerName": "Nathan Reed",
            "reviewerEmail": "nathan.reed@x.dummyjson.com"
        }
    ],
    "returnPolicy": "60 days return policy",
    "minimumOrderQuantity": 12
}
```

**Possible Responses:**

| Status | Meaning |
|---|---|
| `200 OK` | The product with the given ID is returned |
| `404 Not Found` | No product exists with the given ID |

## Search Products by Keyword

**Method:** `GET`
**Path:** `/products/search`

This endpoint returns a paginated list of all products whose title or description matches the given keyword.

Each product object includes fields such as `id`, `title`, `description`, `category`, `price`, `brand`, `tags`, and `reviews`. Several additional fields (`returnPolicy`, `minimumOrderQuantity`, `dimensions`, `meta`, and others) exist on every product but are omitted from the example below for brevity.

**Query Parameters:**

| Parameter | Required | Description |
|---|---|---|
| `q` | No | Keyword to search for, matched against product titles and descriptions. If omitted, the endpoint returns the full product catalog (same behavior as `GET /products`), paginated normally. |

**Response Fields:**

| Field | Description |
|---|---|
| `products` | Array of product objects matching the search keyword, for this page of results |
| `total` | Total number of products matching the search keyword (not the entire catalog) |
| `skip` | Number of matching products skipped before this page begins |
| `limit` | Maximum number of products returned per request |

**Response Example:**

```json
{
  "products": [
    {
      "id": 101,
      "title": "Apple AirPods Max Silver",
      "tags": ["electronics", "over-ear headphones"],
      "reviews": [
        {
          "rating": 5,
          "comment": "Excellent quality!",
          "date": "2025-04-30T09:41:02.053Z",
          "reviewerName": "Henry Adams",
          "reviewerEmail": "henry.adams@x.dummyjson.com"
        }
      ]
    }
  ],
  "total": 1,
  "skip": 0,
  "limit": 1
}
```

**Possible Responses:**

| Status | Meaning |
|---|---|
| `200 OK` | One or more products matched the keyword; they are returned in `products` |
| `200 OK` | No products matched the keyword; `products` is an empty array (`[]`) and `total` is `0` |

## Add a New Product

**Method:** `POST`
**Path:** `/products/add`

This endpoint adds a new product to the catalog. Product details are sent in the request body, and the response echoes back the fields you provided, along with a newly generated `id`. The product object can include any field from `title`, `description`, `category`, `price`, `brand`, `tags`, `reviews`, and others.

> **Note:** DummyJSON is a practice API and does not persist changes. This endpoint returns a realistic-looking response, but the product is not actually saved — a subsequent `GET /products/{id}` call using the returned `id` will return `404 Not Found`.

**Request Body:**

```json
{
  "title": "Blue Hoodie",
  "price": 25
}
```

**Response Example:**

```json
{
    "id": 195,
    "title": "Blue Hoodie",
    "price": 25
}
```

**Possible Responses:**

| Status | Meaning |
|---|---|
| `201 Created` | The product was accepted and a simulated response with a new `id` is returned (not actually saved) |

## Edit Product Details

**Method:** `PUT`
**Path:** `/products/{id}`

**Path Parameters:**

| Parameter | Required | Description |
|---|---|---|
| `id` | Yes | The unique ID of the product to edit |

This endpoint edits the details of the product with the given ID. Only the fields included in the request body are changed — any fields left out are returned unchanged, exactly as they were before (this endpoint behaves like a partial update, not a full replacement, regardless of the `PUT` method name). The response echoes back the fields you sent, along with the rest of the product's existing fields; additional fields are omitted below for brevity.

> **Note:** DummyJSON is a practice API and does not persist changes. This response reflects what an update would look like, but a subsequent `GET /products/{id}` call will show the original, unmodified product.

**Request Body:**

```json
{
  "price": 999
}
```

**Response Example:**

```json
{
  "id": 1,
  "title": "Essence Mascara Lash Princess",
  "price": 999,
  "discountPercentage": 10.48,
  "brand": "Essence",
  "category": "beauty"
}
```

**Possible Responses:**

| Status | Meaning |
|---|---|
| `200 OK` | The product is returned with the sent fields updated (not actually saved) |

## Delete a Product

**Method:** `DELETE`
**Path:** `/products/{id}`

**Path Parameters:**

| Parameter | Required | Description |
|---|---|---|
| `id` | Yes | The unique ID of the product to delete |

This endpoint "deletes" the product with the given ID. Rather than returning an empty response, it returns the complete original product object, with two extra fields added: `isDeleted: true` and a `deletedOn` timestamp.

> **Note:** DummyJSON is a practice API and does not persist changes. This response reflects what a deletion would look like, but a subsequent `GET /products/{id}` call will show the product completely normal, with no `isDeleted` field at all.

**Response Example:**

```json
{
 "id": 1,
 "title": "Essence Mascara Lash Princess",
 "description": "The Essence Mascara Lash Princess is a popular mascara known for its volumizing and lengthening effects. Achieve dramatic lashes with this long-lasting and cruelty-free formula.",
 "category": "beauty",
 "isDeleted": true,
 "deletedOn": "2026-10-01T05:54:11.110Z"
}
```

**Possible Responses:**

| Status | Meaning |
|---|---|
| `200 OK` | The product is returned with `isDeleted: true` and a `deletedOn` timestamp (not actually removed) |
# FastAPI Pagination & Filtering Cheat Sheet

## Basic Query Parameters

``` python
@router.get("/items")
async def list_items(
    limit: int = 20,
    offset: int = 0
):
    ...
```

## With Validation

``` python
from fastapi import Query

limit: int = Query(20, ge=1, le=100)
offset: int = Query(0, ge=0)
```

## Pagination Response Pattern

``` json
{
  "items": [...],
  "limit": 20,
  "offset": 0,
  "total": 134
}
```

## Filtering (Optional Fields)

``` python
@router.get("/books")
async def list_books(
    author: str | None = None,
    published: bool | None = None
):
    ...
```

## Filtering with Enums

``` python
from enum import Enum

class OrderBy(str, Enum):
    created_at = "created_at"
    title = "title"

order_by: OrderBy = OrderBy.created_at
```

## DTO-Based Filters (Recommended)

``` python
class BookFilterDTO(BaseModel):
    author: str | None = None
    min_pages: int | None = None
    max_pages: int | None = None
```

``` python
@router.get("/books")
async def list_books(filters: BookFilterDTO = Depends()):
    ...
```

## Cursor Pagination (Advanced)

``` python
cursor: UUID | None = None
limit: int = Query(20, le=100)
```

## Best Practices

-   Use OFFSET pagination for admin/backoffice
-   Use CURSOR pagination for feeds
-   Always return total count when possible
-   Never expose internal IDs to public users
-   Validate limits to prevent abuse

## HTTP Status Codes

-   200 OK → Paginated results
-   400 Bad Request → Invalid pagination
-   422 Unprocessable Entity → Validation errors

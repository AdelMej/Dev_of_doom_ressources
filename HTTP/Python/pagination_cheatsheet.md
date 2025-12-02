# Pagination Cheat Sheet

## 1. Two Main Types of Pagination

### **Offset Pagination**

-   Uses: `page` + `limit` (e.g., `?page=3&size=20`)
-   Best for: **Stable datasets** (data rarely deleted or reordered)
-   Pros:
    -   Easy to implement
    -   Easy to jump to any page
-   Cons:
    -   Breaks when data is added/removed (page drift)
    -   Slow on huge datasets (big `OFFSET` queries)

### **Cursor-Based Pagination**

-   Uses: `cursor` (unique field like ID/timestamp)
-   Best for: **Volatile datasets** (constant inserts/deletes)
-   Pros:
    -   No page drift
    -   Fast for large data
    -   Great for infinite scroll
-   Cons:
    -   Harder to implement
    -   Cannot reliably jump backward or to page N

------------------------------------------------------------------------

## 2. When to Use What?

  ------------------------------------------------------------------------
  Situation                 Best Method                      Why
  ------------------------- -------------------------------- -------------
  Data is stable and rarely **Offset**                       Simple &
  removed                                                    predictable

  User needs page numbers   **Offset**                       Pages are
                                                             navigable

  Data changes often        **Cursor**                       No missing
  (posts, comments, logs)                                    items / drift

  Need infinite scrolling   **Cursor**                       Natural &
                                                             efficient

  Ordered feed (latest      **Cursor**                       Always
  first)                                                     consistent
  ------------------------------------------------------------------------

------------------------------------------------------------------------

## 3. Expected Behavior

### **Offset + Deletions**

-   Pages may contain **fewer items**
-   This is **expected behavior**
-   Orphans occur unless you keep "soft-deleted" rows (`deleted = true`)

### **Cursor + Deletions**

-   No page drift
-   Client simply continues from last valid cursor

------------------------------------------------------------------------

## 4. What Backend Should Return

### **Offset Pagination Response Example**

``` json
{
  "page": 3,
  "size": 20,
  "totalItems": 154,
  "totalPages": 8,
  "hasNext": true,
  "items": [...]
}
```

### **Cursor Pagination Response Example**

``` json
{
  "nextCursor": "2025-12-01T12:32:55Z",
  "items": [...]
}
```

------------------------------------------------------------------------

## 5. TL;DR

-   **Offset = Easier, predictable, but breaks on volatile data**
-   **Cursor = Harder, but perfect for real-time feeds & infinite
    scroll**

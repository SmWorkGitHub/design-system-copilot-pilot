# Pagination

Requests that return a list of objects *must* return the `items` and `count`
properties in the [Response](#response) to represent the list of objects and
are strongly encouraged to return the `total` property.

APIs that support pagination have a choice of implementing cursor-based or
offset-based pagination, or both of the pagination models.

With cursor-based pagination, the API returns a `next` cursor that designates the end
of the returned page. A client can provide the cursor in the `next` query parameter
in order to retrieve the next page. This model is especially suitable for
implementing "infinite" streams of data where the API client is intended to
fetch in a continuous, forward-moving fashion.

In offset-based pagination, the API client provides an `offset` query parameter in
order to fetch a page that starts from the required offset. This model is
suitable for executing a "random seek" functionality at the cost of potentially
inconsistent results when resource collection is modified between the retrieval
of the pages.

## Query parameters

| Name       | Description                                                          | Must be supported by APIs that support pagination? |
| ---------- | -------------------------------------------------------------------  | -------------------------------------------------- |
| `limit`    | Specifies the number of results to be returned                       |                          Yes                       |
| `next`     | Specifies the pagination cursor for the next page of resources.      | Yes, only if cursor-based pagination is supported  |
| `offset`   | Specifies the zero-based resource offset to start the response from  | Yes, only if offset-based pagination is supported  |

* `offset` must be an integer greater than or equal to zero. Negative or non-integer values must result in a `400 Bad Request` response.
* If neither `offset`, nor `next` are specified, then API must return the first page.
* If both `next` and `offset` query parameters are provided in the same request, then `400 Bad Request` must be returned.
  * Note that this error must be provided even if the API supports both types of pagination and even if `next` and `offset` would separately return the same page.
* In either approaches, API must accept the `limit` query parameter that specifies the requested page size and must document the default that would be applied if `limit` is not specified.
  * Each API that supports pagination must document the maximum value that would be accepted for the `limit`
  * If the requested limit is less than `0` or greater than the documented maximum value, then `400 Bad Request` must be returned.
  * Note that `0` is allowed i.e. the API must succeed providing the correct `total` and other response properties below.

## Response

| Property | Description                               | Required                                                                   |
|----------|-------------------------------------------|----------------------------------------------------------------------------|
| `items`  | The list of objects returned              | Yes                                                                        |
| `count`  | Number of returned items                  | Yes                                                                        |
| `next`   | Cursor for the next page of resources.    | Yes, only if cursor-based pagination is supported                          |
| `total`  | Total number of items in the result set.  | No                                                                         |
| `offset` | Specifies the offset of the returned page | Only required if 'offset' was provided in the query. Encouraged otherwise. |

* For APIs that provide cursor-based pagination, `next` must be `null` if there are no further pages.
* `total` is encouraged for every API that returns a list of resources.
* If the request included filtering such as the [`filter` query parameter](./query_parameters.md#filtering), then the `total` must be the number of items in the result set with the filter applied.
* APIs that support both types of pagination may return both `next` and `offset` per the above rules.

## Examples

### Cursor-based pagination

Fetch the first page

`GET /storage/v1/storage-systems?limit=20`

Response:

```json
{
   "items": [...],
   "count": 20,
   "total": 40,
   "next": "NyBErRdk6czGUyDAX5Cp"
}
```

Fetch the second page

`GET /storage/v1/storage-systems?limit=20&next=NyBErRdk6czGUyDAX5Cp`

Response:

```json
{
   "items": [...],
   "count": 20,
   "total": 40,
   "next": null
}
```

### Offset-based pagination

Fetch the first page

`GET /storage/v1/storage-systems?limit=20`

Response:

```json
{
   "items": [...],
   "count": 20,
   "total": 40,
   "offset": 0,
}
```

Fetch the second page:

`GET /storage/v1/storage-systems?limit=20&offset=20`

Response:

```json
{
   "items": [...],
   "count": 20,
   "total": 40,
   "offset": 20,
}
```

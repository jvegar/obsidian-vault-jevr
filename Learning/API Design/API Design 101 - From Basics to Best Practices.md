## API Design: An E-commerce example

- Consider an e-commerce platform like Shopify
- We're concerned with defining **inputs** and **outputs**
![[Pasted image 20250225074807.png]]

## API Design and CRUD
- Focus is on defining how the CRUD operations are exposed to the user or system interacting with your e-commerce API.
- **CRUD**: Create, Read, Update and Delete
![[Pasted image 20250225074944.png]]

- For example, to add a new product (Create), you would make a POST request to `/api/products` where the product details are sent in the request body.
- To retrieve products, you need to fetch data with GET request to `/products`.
- For updating product information (Update) we use PUT or PATCH request to `/products/:id`
- For removing we use a DELETE request to `/products/:id`

### Communication Protocol and Data Transport Mechanism
- We need to decide on the Communication Protocol like HTTP, Web Sockets, etc., and the Data Transport Mechanism: JSON, XML, or Protocol Buffers.

## API Paradigms
There are different paradigms, each with its own set of protocols and standards.
### REST (Representational State Transfer)
- **Pros**: Stateless: Each request from a client to a server must contain **all** the information needed to understand and complete the request. Use standard HTTP methods (GET, POST, PUT, PATCH, DELETE). Easily consumable by different clients (browsers, mobile apps).
- **Cons**: Can lead to over-fetching or under-fetching of data, because more endpoints may be required to access specific data.
- **Features**: Supports pagination, filtering (limit, offset) and sorting. Uses JSON for data exchange.
### GraphQL
- **Pros**: Allows clients to request exactly what they need, avoiding over-fetching and under-fetching. Strongly typed schema-based queries.
- **Cons**: Complex queries can impact server performance. All request are sent as POST request.
- **Features**: Typically responds with HTTP 200 status code even in case of errors, with error detail in the response body.
### gRPC (Google Remote Procedure Call)
- **Pros**: Built on HTTP/2, which provides advanced features like multiplexing and server push. Uses **Protocol Buffers**, a language-neutral, platform-neutral, extensible way of _serializing_ structured data. Efficient in terms of bandwidth and resources (microservices)
- **Cons**: Less human-readable compared to JSON. Requires HTTP/2 support.
- **Features**: Supports data streaming and bi-directional communication. Ideal for server-to-server communication.

## Relationships in API Design
You might have different relationships: **user to orders**, **order to products**, etc.
![[Pasted image 20250225080719.png]]
Designing endpoints to reflect these relationships is important. For example, **`GET /users/{userID}/orders`** should fetch orders for a specific user.

## Queries, Limit, and Idempotence of GET Requests
Common queries include `limit` and `offset` for pagination or `startDate` and `endDate` for filtering products within a certain range. This allows user to retrieve specific sets of data without overwhelming the system.
![[Pasted image 20250225081109.png]]
A well designed GET request is **idempotent**, meaning calling it multiple times doesn't change the result.

## Backward compatibility and versioning
When modifying endpoints, it's important to maintain backward compatibility. Ensuring changes don't break existing clients

**Versioning**: Introducing versions like (**`/v2/products`**) is a common practice to handle significant changes.
![[Pasted image 20250225081525.png]]
In the case of GraphQL, adding new fields (v2 fields)  without removing old ones helps in evolving the API without breaking existing client.

## Rate Limitations and CORS
- Another best practices is to set rate limitations. This is used to control the number of request a user can make in certain timeframe. It maintains reliability and availability of your API. It also prevents the API from DDoS attacks.
![[Pasted image 20250225083112.png]]


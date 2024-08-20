
### **REST APIs Topics**

1. **RESTful Principles**
   - **REST (Representational State Transfer)** is an architectural style for designing networked applications. RESTful APIs follow a set of principles to ensure scalability, simplicity, and performance.
   - **Key Principles**:
     - **Stateless**: Each API call is independent and does not rely on any previous interaction. The server does not store any session information between requests.
     - **Client-Server**: The client and server are separate entities, allowing them to evolve independently. The client handles the user interface, while the server manages data and business logic.
     - **Uniform Interface**: The API should have a consistent and well-defined interface, making it easy for clients to understand and use. This includes using standard HTTP methods (GET, POST, PUT, DELETE) and consistent resource URIs.
     - **Resource-Based**: Everything in REST is considered a resource, and each resource is identified by a URI. Resources are manipulated using standard HTTP methods.
     - **Layered System**: A RESTful system can be composed of multiple layers (e.g., load balancers, proxies, authentication layers) that interact with each other without affecting the overall system behavior.
     - **Cacheable**: Responses from the server should be marked as cacheable or non-cacheable to improve performance by reducing the need for repeated requests.

2. **API Design**
   - Good API design focuses on simplicity, consistency, and ease of use. Key considerations include:
     - **Resource Naming**: Use nouns to represent resources (e.g., `/users`, `/orders`). Avoid verbs in the endpoint paths.
     - **HTTP Methods**: Use the appropriate HTTP methods for each action:
       - **GET**: Retrieve data from the server.
       - **POST**: Create a new resource.
       - **PUT**: Update an existing resource.
       - **DELETE**: Remove a resource.
     - **Status Codes**: Use standard HTTP status codes to indicate the outcome of an API request (e.g., `200 OK`, `201 Created`, `404 Not Found`, `500 Internal Server Error`).
     - **Error Handling**: Provide meaningful error messages with relevant status codes to help clients understand what went wrong.

3. **Versioning**
   - API versioning allows you to introduce changes to an API without breaking existing clients. Common versioning strategies include:
     - **URI Versioning**: Embed the version number in the URI (e.g., `/api/v1/users`). This is the most explicit method and makes it clear which version of the API is being used.
     - **Query Parameters**: Include the version number as a query parameter (e.g., `/users?version=1`).
     - **Header Versioning**: Specify the version number in the HTTP headers (e.g., `Accept: application/vnd.example.v1+json`).
     - **Content Negotiation**: Use the `Accept` header to specify the desired version, allowing the server to respond with the appropriate version.

4. **Authentication (OAuth2, JWT)**
   - **OAuth2**: A widely-used authorization framework that allows third-party applications to access a user's resources without exposing their credentials. OAuth2 uses tokens to grant access and is often used for delegated access (e.g., logging in with Google or Facebook).
     - **OAuth2 Flow**: Involves several steps, including obtaining an authorization code, exchanging it for an access token, and using the token to access protected resources.
     - **Scopes**: Define the level of access granted by the token.
   - **JWT (JSON Web Token)**: A compact, self-contained token format used for authentication and information exchange. JWTs are signed, ensuring data integrity and authenticity.
     - **Structure**: Consists of three parts: Header, Payload, and Signature. The payload contains claims, which can include user information and token expiration details.
     - **Use Cases**: JWT is commonly used for stateless authentication in APIs, where the token is passed with each request to verify the user's identity.

5. **Rate Limiting**
   - **Rate Limiting**: A technique used to control the number of requests a client can make to an API within a specific time frame. It helps protect the API from abuse and ensures fair usage.
   - **Implementation Strategies**:
     - **Fixed Window**: Limits requests within a fixed time window (e.g., 100 requests per minute).
     - **Sliding Window**: Allows more fine-grained control by considering the time elapsed since the first request in a set of requests.
     - **Token Bucket**: Assigns a "bucket" of tokens to each client. Each request consumes a token, and tokens are replenished at a fixed rate.
     - **Leaky Bucket**: Similar to the token bucket, but tokens leak out of the bucket at a fixed rate, preventing sudden spikes in requests.

---

### **Answers to the Questions**

1. **What are the key principles of REST?**
   - The key principles of REST include:
     - **Stateless**: Each request from a client to the server must contain all the information needed to understand and process the request. The server does not store any session data.
     - **Client-Server**: The client and server are independent; the client only needs to know the URI of the resource, while the server provides the requested resources.
     - **Uniform Interface**: REST APIs must have a consistent interface. This involves using standard HTTP methods, URIs to identify resources, and consistent response formats.
     - **Resource-Based**: Resources are the fundamental units in REST, identified by URIs. Operations on resources are performed using standard HTTP methods.
     - **Layered System**: REST architecture can have multiple layers between the client and the server, such as caching layers, security layers, or load balancers, without the client knowing.
     - **Cacheable**: Responses should define whether they are cacheable or not, to improve performance by reducing the need for repeated requests.

2. **How do you handle versioning in APIs?**
   - **URI Versioning**: Embed the version number directly in the URI (e.g., `/api/v1/products`). This is clear and explicit, making it easy for clients to switch between versions.
   - **Query Parameters**: Include the version number as a query parameter (e.g., `/products?version=1`). This method is flexible but less explicit than URI versioning.
   - **Header Versioning**: Use a custom HTTP header to specify the API version (e.g., `Accept: application/vnd.company.v1+json`). This keeps the URI clean and allows versioning to be handled in a more flexible manner.
   - **Content Negotiation**: Use the `Accept` header to specify the desired version, allowing the server to return the appropriate version of the resource. This method supports backward compatibility and is flexible, but can be more complex to implement.

3. **Explain the differences between OAuth2 and JWT.**
   - **OAuth2**:
     - **Authorization Framework**: OAuth2 is a protocol designed for delegated access, where a third-party application is granted limited access to a user's resources without exposing their credentials.
     - **Token Types**: OAuth2 typically issues two types of tokens: access tokens (short-lived) and refresh tokens (longer-lived) that can be used to obtain new access tokens.
     - **Flow**: OAuth2 involves multiple steps, including authorization, token exchange, and token validation. It is used for scenarios like logging in with a social media account.
   - **JWT**:
     - **Token Format**: JWT is a compact, URL-safe token format used for transmitting information between parties. It is typically used for stateless authentication.
     - **Self-Contained**: JWT tokens contain all the necessary information (claims) within the token itself, including user identity and token expiration.
     - **Structure**: JWT consists of three parts: Header (metadata), Payload (claims), and Signature (used to verify the token’s integrity).
     - **Use Case**: JWT is often used in stateless authentication systems where the token is sent with each request, allowing the server to authenticate the user without maintaining session state.

4. **How would you implement rate limiting for an API?**
   - **Using Middleware**: Implement rate limiting in your API by using middleware that tracks the number of requests per client (usually based on the client's IP address or API key).
     - **Example in Django**:
       ```python
       from django.core.cache import cache
       from django.http import JsonResponse

       def rate_limit_middleware(get_response):
           def middleware(request):
               client_ip = request.META['REMOTE_ADDR']
               key = f'rate_limit_{client_ip}'
               request_count = cache.get(key, 0)

               if request_count >= 100:  # Limit of 100 requests per minute
                   return JsonResponse({'error': 'Rate limit exceeded'}, status=429)

               cache.set(key, request_count + 1, timeout=60)  # Reset count every 60 seconds
               return get_response(request)

           return middleware
       ```
   - **Using API Gateway**: If you're deploying your API on cloud platforms like AWS, GCP, or Azure, you can configure rate limiting in the API gateway settings, which handles rate limiting automatically without modifying your application code.
   - **Token Bucket/Leaky Bucket Algorithms**: Implement advanced rate limiting strategies using algorithms that manage request tokens or leaks, allowing for more flexible and fair usage.


Caching is a common technique that aims to improve the performance and scalability of a system. It does this by temporarily copying frequently accessed data to fast storage that's located close to the application. If this fast data storage is located closer to the application than the original source, then caching can significantly improve response times for client applications by serving data more quickly.

Azure provides 2 standard features for caching:

1. For backend caching a Redis Cache Service can be used
2. For frontend caching a Content Delivery Network (CDN) can be used
    - For caching static files that doesnt change like images, videos, css, html etc.

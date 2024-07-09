## E-Tags(Entity Tags)

---> Entity tags (ETags) are HTTP response headers that can be used to improve the efficiency of web applications by reducing unnecessary data transfers and ensuring data consistency. ETags can be used for caching and conditional requests. 

---> In conditional requests, clients can include the ETag received in a previous response in subsequent requests using the header "If-None-Match". The API will calculate the ETag again and compare it to the value sent by the client. If the ETag values match, the API will respond with a 304 Not Modified status, which means the response is the same and no response will be sent back to the client. This saves bandwidth and reduces server load. The client can then update its cache, knowing that the previously cached response is still valid. 

---> ETag values are typically hashes of the content, the last modification timestamp, or a revision number. They are strings of ASCII characters placed between double quotes, like "675af34563dc-tr34". The ETag algorithm family is set to MD5up by default, but it can be configured differently.

<img width="703" alt="Screenshot 2024-07-09 at 2 19 34 PM" src="https://github.com/mkathiravan/Android-Notes/assets/39657409/15a293df-9fca-4d46-ad63-04b0c04f4368">

---> ETags or Entity Tags are part of the HTTP protocol and provide a mechanism for web cache validation and to manage concurrent changes to a resource. In REST APIs, ETags help improve the efficiency of web applications by reducing unnecessary data transfers and ensuring the consistency of the data being accessed. This blog post explores the concept of ETags, and their benefits in REST APIs, and provides a practical implementation example using Spring Boot and Java.

#### What is an ETag?

---> An ETag (Entity Tag) is a unique identifier assigned by a web server to a specific version of a resource. It is sent in the HTTP headers. When a resource is changed, its ETag changes, enabling clients to detect changes. This mechanism supports two main features:

    i) Cache Validation: It allows clients to make conditional requests asking for the resource only if it has changed, thereby saving bandwidth and reducing load on the server.
    
    ii) Concurrency Control: It prevents accidental overwrites when multiple clients are modifying the same resource concurrently, a strategy known as “optimistic concurrency control”.


  #### How ETags Work?

  ---> **1. First Request and Response**: A client makes an HTTP GET request for a resource. The server responds with the resource, including an ETag in the response headers.
  
  ----> **2. Subsequent Requests**: When the client needs to request the same resource again, it can send the previous ETag values in an If-None-Match HTTP header. 

  The server checks this ETag:
  
a. If the ETag matches the current version of the resource, it means there has been no change, and the server returns a 304 Not Modified status, without a body.

b. If the ETag does not match, it indicates the resource has changed, and the server sends the new resource with a 200 OK status along with the updated ETag.

 ---> **3. Concurrency with PUT Requests**: When updating a resource, a client includes the ETag in an If-Match header. The server only updates the resource if the ETag matches the current resource state, ensuring no other changes were made since the last fetch.

# Kusuma Ratih Hanindyani - 2306256406

## Reflection - Module 8

1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

   - Unary RPC involves a single request from the client and a single response from the server. It's ideal for straightforward operations like fetching user details.
   - Server Streaming RPC allows the client to send one request and receive a stream of responses. This is suitable for scenarios like retrieving a list of items or real-time data feeds.
   - Bidirectional Streaming RPC enables both client and server to send and receive streams of messages independently. This is particularly effective for real-time applications like chat systems, where continuous two-way communication is essential .


2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

   - Authentication: ensuring that only legitimate clients and servers can communicate
   - Authorization: controlling access based on roles or permissions after authentication
   - Encryption: gRPC over HTTP/2 supports TLS by default, which protects the payload and metadata from eavesdropping or tampering.


3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

   - Concurrency Management: Two independent tasks (reading and writing) need to be executed simultaneously with caution using `tokio::spawn`, and misusing them can result in deadlocks or panics.
   - Backpressure and Flow Control: The streams need to handle cases where one streams faster than the other can process which may cause memory pressure or dropped messages.
   - Client Disconnections: Disconnections are handled gracefully by monitoring the stream state and terminating the orphaned tasks.
   - Error Propagation: The errors from streams must be exposed and managed without crashing the service.
    

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

    Advantages:
    - Simplifies streaming by bridging a `tokio::mspc::Receiver` into a `Stream`
    - Encourages clean separation of logic, where message production is decoupled from transmission
    - Non-blocking and integrates naturally into the async model

    Disadvantages:
   - Fixed buffer size may drop messages if the consumer lags.
   - Sender must manually handle slow consumers
   - Less ideal for high-throughput or low-latency systems where more advanced stream management is required


5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

   - Separate services into modules to isolate logic.
   - Use traits for service implementations, allowing mocking and unit testing.
   - Define shared types in a common crate or module.
   - Extract repetitive patterns into utilities.
   - Consider `build.rs` for shared proto handling and custom client generation.


6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

   - Transaction Management: Implementing atomic operations to ensure consistency.
   - Integration with External Systems: Communicating with third-party payment gateways.
   - Enhanced Security: Incorporating fraud detection and regulatory compliance.
   - Input Validation: Verifying and sanitizing incoming payment data.
   - Error Recovery: Adding retry logic and graceful handling of failures.


7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

    Adopting gRPC improves performance by leveraging HTTP/2 and Protocol Buffers for efficient communication. It enhances interoperability by enabling services written in different programming languages to communicate seamlessly. Additionally, gRPC encourages contract-first development by promoting the use of clearly defined interfaces for consistent and reliable service interactions.


8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

    Advantages:
   - Multiplexing: Allows multiple streams over a single connection, reducing latency.
   - Header Compression: Decreases overhead, improving performance.

    Disadvantages:
   - Complexity: More intricate implementation compared to HTTP/1.1.
   - Limited Browser Support: Direct gRPC over HTTP/2 is not natively supported in browsers, necessitating workarounds like gRPC-Web .


9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

   REST's stateless request-response model is straightforward but less suited for real-time communication. In contrast, gRPC's bidirectional streaming enables persistent connections, allowing for low-latency, real-time data exchange, which is essential for applications like live chats and streaming services.


10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

    The schema-based approach of gRPC, using Protocol Buffers, enforces strict data contracts, resulting in more reliable and efficient communication. However, it is less human-readable and requires additional tools for inspection. In contrast, the schema-less nature of JSON in REST APIs offers flexibility and human-readability, making it easier for rapid development, but this can lead to inconsistencies and runtime errors due to the lack of enforced structure.
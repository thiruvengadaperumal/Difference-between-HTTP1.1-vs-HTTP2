# Difference-between-HTTP1.1-vs-HTTP2
HTTP1 VS HTTP2
Compression
A common method of optimizing web applications is to use compression algorithms to reduce the size of HTTP messages that travel between the client and the server. HTTP/1.1 and HTTP/2 both use this strategy, but there are implementation problems in the former that prohibit compressing the entire message. The following section will discuss why this is the case, and how HTTP/2 can provide a solution.

HTTP/1.1
Programs like gzip have long been used to compress the data sent in HTTP messages, especially to decrease the size of CSS and JavaScript files. The header component of a message, however, is always sent as plain text. Although each header is quite small, the burden of this uncompressed data weighs heavier and heavier on the connection as more requests are made, particularly penalizing complicated, API-heavy web applications that require many different resources and thus many different resource requests. Additionally, the use of cookies can sometimes make headers much larger, increasing the need for some kind of compression.

In order to solve this bottleneck, HTTP/2 uses HPACK compression to shrink the size of headers, a topic discussed further in the next section.

HTTP/2
One of the themes that has come up again and again in HTTP/2 is its ability to use the binary framing layer to exhibit greater control over finer detail. The same is true when it comes to header compression. HTTP/2 can split headers from their data, resulting in a header frame and a data frame. The HTTP/2-specific compression program HPACK can then compress this header frame. This algorithm can encode the header metadata using Huffman coding, thereby greatly decreasing its size. Additionally, HPACK can keep track of previously conveyed metadata fields and further compress them according to a dynamically altered index shared between the client and the server. For example, take the following two requests:

Request #1
method:		GET
scheme:		https
host:		example.com
path:		/academy
accept:		/image/jpeg
user-agent:	Mozilla/5.0 ...
Request #2
method:		GET
scheme:		https
host:		example.com
path:		/academy/images
accept:		/image/jpeg
user-agent:	Mozilla/5.0 ...
The various fields in these requests, such as method, scheme, host, accept, and user-agent, have the same values; only the path field uses a different value. As a result, when sending Request #2, the client can use HPACK to send only the indexed values needed to reconstruct these common fields and newly encode the path field. The resulting header frames will be as follows:

Header Frame for Request #1
method:		GET
scheme:		https
host:		example.com
path:		/academy
accept:		/image/jpeg
user-agent:	Mozilla/5.0 ...
Header Frame for Request #2
path:		/academy/images
Using HPACK and other compression methods, HTTP/2 provides one more feature that can reduce client-server latency.

Conclusion
As you can see from this point-by-point analysis, HTTP/2 differs from HTTP/1.1 in many ways, with some features providing greater levels of control that can be used to better optimize web application performance and other features simply improving upon the previous protocol. Now that you have gained a high-level perspective on the variations between the two protocols, you can consider how such factors as multiplexing, stream prioritization, flow control, server push, and compression in HTTP/2 will affect the changing landscape of web development.

If you would like to see a performance comparison between HTTP/1.1 and HTTP/2, check out this Google demo that compares the protocols for different latencies. Note that when you run the test on your computer, page load times may vary depending on several factors such as bandwidth, client and server resources available at the time of testing, and so on. If you’d like to study the results of more exhaustive testing, take a look at the article HTTP/2 – A Real-World Performance Test and Analysis. Finally, if you would like to explore how to build a modern web application, you could follow our How To Build a Modern Web Application to Manage Customer Information with Django and React on Ubuntu 18.04 tutorial, or set up your own HTTP/2 server with our How To Set Up Nginx with HTTP/2 Support on Ubuntu 20.04 tutorial.

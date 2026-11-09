# Lab 01: File Path Traversal (Simple Case)

## 🎯 Objective
To read the contents of the `/etc/passwd` file from the server by exploiting a path traversal vulnerability in the product image loading functionality.

## 🛠️ Environment & Tools Used
*   **Target:** PortSwigger Web Security Academy (Lab Instance)
*   **Tool:** Burp Suite Professional (Proxy & Repeater)

## 🔍 Vulnerability Analysis
The application uses a `filename` parameter to retrieve images from the server (e.g., `GET /image?filename=60.jpg`). The application fails to sanitize user input, allowing the use of relative path sequences (like `../`) to traverse outside the intended image directory.

## 💥 Exploitation Steps

### 1. Intercept the Request
While browsing the website, I enabled the Burp Suite Proxy and intercepted the request for a product image. The HTTP History tab shows the initial request for the image `60.jpg`:

![Burp Suite Proxy History](./burp-history.png)

### 2. Modify the Payload
I sent this request to the **Repeater** and modified the `filename` parameter to traverse up the directory tree to reach the root directory and access the sensitive `/etc/passwd` file:

```http
GET /image?filename=../../../../etc/passwd HTTP/2
Host: [lab-id].web-security-academy.net

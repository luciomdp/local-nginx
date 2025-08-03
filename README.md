# Local NGINX Reverse Proxy for Development

This project sets up an NGINX reverse proxy using Docker, allowing local development with custom domains under the `*.local.dev` namespace. It simulates a production-like environment by redirecting requests made to these local domains to the correct services, helping developers work with real-looking URLs locally (e.g., `app.local.dev`, `api.local.dev`).

## Features

- Reverse proxy via NGINX  
- HTTPS with a self-signed certificate (`local-dev.crt`)  
- Easy setup with Docker and Docker Compose  
- Works on macOS and Windows  

## Requirements

- [Docker](https://www.docker.com/products/docker-desktop) installed and running  
- Access to modify trusted certificates on your machine  
- A browser restart after adding the certificate  

## Setup Instructions

### 1. Import the SSL Certificate

The proxy uses `local-dev.crt` as a self-signed certificate. You must trust this certificate locally for HTTPS to work without warnings.

#### macOS

1. Open the **Keychain Access** app.  
2. Drag and drop `sslkeys/local-dev.crt` into the **System** keychain.  
3. Double-click the certificate, expand **Trust**, and set **When using this certificate** to **Always Trust**.  
4. Close the window and enter your password to save changes.  
5. **Restart your browser**.  

#### Windows

1. Press `Win + R`, type `mmc`, and hit Enter.  
2. Go to **File > Add/Remove Snap-in...**, select **Certificates**, click **Add**, and choose **Computer account**.  
3. Navigate to **Trusted Root Certification Authorities > Certificates**.  
4. Right-click and choose **All Tasks > Import**, then import `sslkeys/local-dev.crt`.  
5. Complete the wizard and click **Finish**.  
6. **Restart your browser**.  

### 2. Start the Proxy

In the root directory of the project (where `docker-compose.yml` is located), run:

```bash
docker-compose up
```
This will build and start the NGINX reverse proxy container with the specified config and mounted SSL certificates.

### 3. Use It in Development  
Now you can visit your services using custom domains like:
```bash
https://app.local.dev
https://api.local.dev
```
Make sure to map these domains properly in your `nginx_config` or local DNS setup, for example by editing your `/etc/hosts` file or using a local DNS service.

---

## Notes

- This is **only for development**; do not use the self-signed certificate in production.  
- The setup mimics production HTTPS behavior to reduce bugs caused by differences between local and production environments.

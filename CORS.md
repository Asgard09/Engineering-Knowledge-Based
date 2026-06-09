**CORS** stands for **Cross-Origin Resource Sharing**. It is a security mechanism implemented by your **web browser** that controls how a frontend application running on one website can talk to a backend API running on a completely different website.

By default, the Same-Origin Policy strictly forbids a frontend script on `myfrontend.com` from fetching data from `myapi.com`. This prevents malicious websites from writing scripts to steal data from your banking or email APIs.

However, in modern web development, we want our frontend (e.g., a React app on port 3000) to talk to our backend (e.g., a Spring Boot API on port 8080). This is where **CORS** comes in. It is a way for the backend to tell the browser: "Hey, I trust this specific frontend. Let them in!"
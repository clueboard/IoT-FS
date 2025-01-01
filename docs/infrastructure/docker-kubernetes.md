# Server(s) Using Kubernetes

Kubernetes, or K8s, is an open-source system for automating deployment, scaling, and management of containerized applications. What does that mean? It will manage your containers for you. Virtually every company that uses containers has settled on K8s as the way to manage the deployment and lifecycle of their applications.

Should you use K8s for your IoT-FS infrastructure? Ye, if you want to learn K8s. If you're not interested in learning k8s there's not much reason to set it up. There is a lot of work that goes into installing and using K8s, and you will have a lot of concepts to learn. If you are running a handful of services that you never have to scale, which is by far the most common IoT-FS situation, the sweet spot is one or two servers running Docker with Docker Compose.

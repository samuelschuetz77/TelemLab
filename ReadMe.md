### This Repo's purpose
This repo builds a network of lab machines that each run a local AI model backend. When clients talk to the model, the request can be routed to any available backend on any machine in the class.

Each machine in the lab should run:
- OpenWebUI or another frontend that can talk to local or remote OpenAI-compatible backends
- `llama-cpp` as the local model server
- `nvidia-exporter` so Prometheus can scrape GPU metrics from the machine

### Central monitoring
One designated machine should run the central Prometheus and Grafana stack. That machine is responsible for collecting GPU data from all student machines.

Prometheus needs a configuration file (`prometheus.yml`) with the IP addresses of every machine running `nvidia-exporter`.
Grafana needs to be connected to that Prometheus instance as a data source, and you can import an NVIDIA GPU dashboard to visualize GPU usage and request load.

### OpenWebUI routing
OpenWebUI supports multiple OpenAI API backend connections in its settings. You can add other lab machines manually using connection strings like:
- `http://192.168.1.10:8080/v1`
- `http://192.168.1.11:8080/v1`

For a more automated setup, place an HTTP load balancer such as Nginx or HAProxy in front of the lab backends and point OpenWebUI at the load balancer.



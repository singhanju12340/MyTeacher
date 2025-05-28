Its a service mesh.

all traffic flows through it. This allows it to grab metrics, enable / disable encryption, security policies, inject / remove headers, add authentication and most importantly make routing discussions . Unlike an ingress controller, istio works by attaching itself to a kubernetes pod on startup. This means pod to pod communication will be analyzed because istio is not depending on an edge router.


A lot of people run istio to get observability into their cluster, but the main focus of istio is to do advance routing, canary deployment, traffic mirroring etc. This can be done WITHOUT affecting the upstream app or adding configuration to the docker container.
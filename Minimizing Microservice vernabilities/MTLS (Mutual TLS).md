let consider we have 2 pod, If the communication between 2 pods are not encrypted then hacker can break in to network and see the sensitive information. when we make any request to secure website then that is the one way or single tls. when pods communicating both need to share the data.

Mutual TLS ensure the communication between both pods are encrypted. 
but when we use different type of pod like MySQL or Nginx both have different encryption algorithm. to solve this ==istio== by running a sidecar container encrypt and de-crypt the data for both pods.


To enable the mtls with istio check below link
https://istiobyexample.dev/mtls/

check below for more about istio sidecar container.
https://istio.io/latest/docs/setup/getting-started/
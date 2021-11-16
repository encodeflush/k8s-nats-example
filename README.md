# k8s-nats-example

```bash
k create ns nats
helm install nats -n nats chart/nats
```

NATS can be accessed via port 4222 on the following DNS name from within your cluster:

`nats-client.nats.svc.cluster.local`

We will expose this service on the localhost:

```bash
k -n nats port-forward svc/nats-client 4222:4222
Forwarding from 127.0.0.1:4222 -> 4222
Forwarding from [::1]:4222 -> 4222
```

To get the authentication credentials, run:

    export NATS_USER=$(kubectl get cm --namespace nats nats -o jsonpath='{.data.*}' | grep -m 1 user | awk '{print $2}' | tr -d '"')
    export NATS_PASS=$(kubectl get cm --namespace nats nats -o jsonpath='{.data.*}' | grep -m 1 password | awk '{print $2}' | tr -d '"')
    echo -e "Client credentials:\n\tUser: $NATS_USER\n\tPassword: $NATS_PASS"
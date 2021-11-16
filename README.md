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

```bash 
export NATS_USER=$(kubectl get cm --namespace nats nats -o jsonpath='{.data.*}' | grep -m 1 user | awk '{print $2}' | tr -d '"')
export NATS_PASS=$(kubectl get cm --namespace nats nats -o jsonpath='{.data.*}' | grep -m 1 password | awk '{print $2}' | tr -d '"')
echo -e "Client credentials:\n\tUser: $NATS_USER\n\tPassword: $NATS_PASS"
```

You can intall NATS client locally and run a simple test:

```bash
GO111MODULE=off go get github.com/nats-io/nats.go
cd $GOPATH/src/github.com/nats-io/nats.go/examples/nats-pub && go install && cd
cd $GOPATH/src/github.com/nats-io/nats.go/examples/nats-echo && go install && cd
python client/sample.py
Received a message on 'bar ': First
Received a message on 'bar ': Second
Received a message on 'help _INBOX.QLU7SyP1GLBbD9kwsv0mM5.QLU7SyP1GLBbTEkwsv0mM5': help me
Received response: help me
```
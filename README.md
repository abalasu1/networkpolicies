# networkpolicies

# Backend
kubectl apply -f https://raw.githubusercontent.com/abalasu1/networkpolicies/master/backend.yaml
kubectl get pods -n calc-backend --show-labels

# Frontend
kubectl apply -f https://raw.githubusercontent.com/abalasu1/networkpolicies/master/frontend.yaml
kubectl get pods -n calc-frontend --show-labels

# Debug
kubectl apply -f https://raw.githubusercontent.com/abalasu1/networkpolicies/master/debug.yaml
kubectl get pods -n debug --show-labels

# Check namespace labels
kubectl get ns --show-labels

# Apply backend policies
kubectl apply -f https://raw.githubusercontent.com/abalasu1/networkpolicies/master/backend-networkpolicies.yaml

# Apply frontend policies
kubectl apply -f https://raw.githubusercontent.com/abalasu1/networkpolicies/master/frontend-networkpolicies.yaml

# Test scripts
kubectl exec -it $(kubectl get pods -l app=cmdpodbe -o jsonpath="{.items[].metadata.name}" -n calc-backend) -n calc-backend bash

kubectl exec -it $(kubectl get pods -l app=cmdpodfe -o jsonpath="{.items[].metadata.name}" -n calc-frontend) -n calc-frontend bash

kubectl exec -it $(kubectl get pods -l app=curl -o jsonpath="{.items[].metadata.name}" -n debug) -n debug bash

curl --max-time 1 "http://svc-add.calc-backend:8080/basicop/add?n1=100&n2=200"

curl --max-time 1 "http://svc-subtract.calc-backend:8080/basicop/subtract?n1=200&n2=100"

curl --max-time 1 "http://svc-multiply.calc-backend:8080/basicop/multiply?n1=10&n2=20"

curl --max-time 1 "http://svc-divide.calc-backend:8080/basicop/divide?n1=200&n2=100"

curl --max-time 1 "http://svc-compositeop.calc-backend:8080/compositeop/eval?expr=(10*3)%2B5-10/5"
curl --max-time 10 -X POST http://svc-processor.calc-frontend:8080/processor/calculate?verbosity=1 -H 'Content-Type: application/json' -d '{
    "x1": "2 + 3 + 4",
    "x2": "3 - 2",
    "x3": "3 * 2",
    "x4": "4 / 2",
    "x5": "1 + 2 + 3 + 4 + 33 - 6 - 7 * 3 + 12 / 4",
    "x6": "12 / 4 / 3"
}'

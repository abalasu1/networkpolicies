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


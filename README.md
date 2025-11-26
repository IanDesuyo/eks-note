# eks-note

## Without Auto Mode

- Install Amazon EKS Pod Identity Agent add-on when using Pod Identity
- Create OIDC serviceaccount for LB Controller
- Install Loadbalancer Controller via Helm

[NLB](https://docs.aws.amazon.com/eks/latest/userguide/network-load-balancing.html): service class (More Eazy)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nlb-sample-service
  namespace: nlb-sample-app
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-type: external
    service.beta.kubernetes.io/aws-load-balancer-nlb-target-type: ip
    service.beta.kubernetes.io/aws-load-balancer-scheme: internet-facing
spec:
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
  type: LoadBalancer
  selector:
    app: nginx
```
[ALB](https://docs.aws.amazon.com/eks/latest/userguide/alb-ingress.html): ingress class
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nlb-sample-service
  namespace: nlb-sample-app
spec:
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
  type: NodePort
  selector:
    app: nginx
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nlb-sample-ingress
  namespace: nlb-sample-app
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
spec:
  ingressClassName: alb
  rules:
    - http:
        paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: nlb-sample-service
              port:
                number: 80
```

## With Auto Mode

- Install Amazon EKS Pod Identity Agent add-on when using Pod Identity
- Don't need to install Loadbalancer controller

[NLB](https://docs.aws.amazon.com/eks/latest/userguide/auto-configure-nlb.html): service class (More Eazy)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nlb-sample-service
  namespace: nlb-sample-app
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-scheme: internet-facing
spec:
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
  type: LoadBalancer
  selector:
    app: nginx
  loadBalancerClass: eks.amazonaws.com/nlb
```
[ALB](https://docs.aws.amazon.com/eks/latest/userguide/auto-configure-alb.html): ingress class (Too hard)

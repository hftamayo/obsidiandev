
![[Pasted image 20240220152455.png]]

microk8s.kubectl exec --namespace jenkins -it svc/jenkins -c jenkins -- /bin/cat /run/secrets/additional/chart-admin-password && echo

el usuario default es: admin
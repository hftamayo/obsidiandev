
For production quality, I’d do:
playbooks/dev-vm-bootstrap.yml
roles/dev_vm_bootstrap/
tasks/prereqs.yml
tasks/k3s.yml
tasks/tools.yml
tasks/verify.yml

I’d probably add one more task later to confirm the node is actually Ready, not just that get nodes succeeds. That can live in verify.yml.

tasks/prereqs.yml
Handles host prep.
Tasks
update package metadata
install dependencies like curl, tar, gzip, iptables, socat, jq, conntrack tools
install any prerequisites needed by k3s
make sure the host is ready for installation
Output
The VM can safely install k3s.

tasks/k3s.yml
Handles k3s installation and service startup.
Tasks
install k3s using the desired version
configure node name
enable systemd service
restart/start k3s
wait for the service to become active
Output
k3s.service is running.

tasks/tools.yml
Handles user tools.
Tasks
ensure kubectl is available
ensure kubeconfig is usable
install Helm if needed
make sure path/location is correct for the intended user or system context
Output
You can run kubectl and helm as expected.

tasks/verify.yml
Handles readiness checks.
Tasks
wait for API readiness
verify node is Ready
verify k3s kubectl get nodes succeeds
verify plain kubectl works in the intended context
verify Helm is installed if it’s part of the contract
Output
The VM-ready acceptance criteria are satisfied.

Recommended execution order
prereqs
k3s
tools
verify
That maps directly to your success contract and keeps failures easy to diagnose.


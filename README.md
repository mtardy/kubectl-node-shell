## Differences of my fork

This is a fork of https://github.com/kvaps/kubectl-node-shell to add two
functionalities:
- Be able to use the script from inside a pod (i.e. with a service account
  token mounted and no kubeconfig).
- Select a random node when you don't specify it in the command line (because
  often, roles tied to service account have no specific permissions about
  nodes).

To use the custom feature and nsenter all the namespace on a random node, just
use the `--incluster` flag:
```bash
kubectl node-shell --incluster
```

# kubectl node-shell
*(formerly known as **kubectl-enter**)*

Start a root shell in the node's host OS running.

![demo](https://gist.githubusercontent.com/kvaps/2e3d77975a844654ec297893e21a0829/raw/c778a8405ff8c686e4e807a97e9721b423e7208f/kubectl-node-shell.gif)

## Installation

using [krew](https://krew.sigs.k8s.io/):

<pre>
kubectl krew index add kvaps <a href="https://github.com/kvaps/krew-index">https://github.com/kvaps/krew-index</a>
kubectl krew install kvaps/node-shell
</pre>

or using curl:

```bash
curl -LO https://github.com/kvaps/kubectl-node-shell/raw/master/kubectl-node_shell
chmod +x ./kubectl-node_shell
sudo mv ./kubectl-node_shell /usr/local/bin/kubectl-node_shell
```

## Usage

```bash
# Get standard bash shell
kubectl node-shell <node>

# Execute custom command
kubectl node-shell <node> -- echo 123

# Use stdin
cat /etc/passwd | kubectl node-shell <node> -- sh -c 'cat > /tmp/passwd'

# Run oneliner script
kubectl node-shell <node> -- sh -c 'cat /tmp/passwd; rm -f /tmp/passwd'
```

*You need to be able to start privileged containers for that.*

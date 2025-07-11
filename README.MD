
# MY CAPSTONE PROJECT-4

First, I installed the Linode CLI using Homebrew:

```sh
sgworker@MacBook-Pro-3.local /Users/sgworker/Desktop/CAPSTONE-PROJECT-4 
% brew install linode-cli

==> Auto-updating Homebrew...
Adjust how often this is run with HOMEBREW_AUTO_UPDATE_SECS or disable with
HOMEBREW_NO_AUTO_UPDATE. Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
==> Auto-updated Homebrew!
..............

```
After that, I created my cluster on linode with the Linode CLI:
```sh
sgworker@MacBook-Pro-3.local /Users/sgworker/Desktop/CAPSTONE-PROJECT-4 
% linode-cli lke cluster-create \
  --label my-linode-cluster \
  --region eu-west \
  --k8s_version 1.28 \
  --node_pools.type g6-standard-2 \
  --node_pools.count 2

Using default values: {}; use the --no-defaults flag to disable defaults
Request failed: 400
errors                                    
┌─────────────┬──────────────────────────┐
│ field       │ reason                   │
├─────────────┼──────────────────────────┤
│ k8s_version │ k8s_version is not valid │
└─────────────┴──────────────────────────┘
sgworker@MacBook-Pro-3.local /Users/sgworker/Desktop/CAPSTONE-PROJECT-4 
% linode-cli lke versions-list

┌──────┐
│ id   │
├──────┤
│ 1.33 │
├──────┤
│ 1.32 │
├──────┤
│ 1.31 │

sgworker@MacBook-Pro-3.local /Users/sgworker/Desktop/CAPSTONE-PROJECT-4 
% linode-cli lke cluster-create \
  --label my-linode-cluster \
  --region eu-central \
  --k8s_version 1.31 \
  --node_pools.type g6-standard-2 \
  --node_pools.count 2
Using default values: {}; use the --no-defaults flag to disable defaults
┌────────┬───────────────────┬────────────┬─────────────┬─────────────────────────────────┬──────┐
│ id     │ label             │ region     │ k8s_version │ control_plane.high_availability │ tier │
├────────┼───────────────────┼────────────┼─────────────┼─────────────────────────────────┼──────┤
│ 497779 │ my-linode-cluster │ eu-central │ 1.31        │ False                           │      │
└────────┴───────────────────┴────────────┴─────────────┴─────────────────────────────────┴──────┘


```


```sh
sgworker@MacBook-Pro-3.local /Users/sgworker/Desktop/CAPSTONE-PROJECT-4 
% export KUBECONFIG=my-linode-cluster-kubeconfig.yaml 
```




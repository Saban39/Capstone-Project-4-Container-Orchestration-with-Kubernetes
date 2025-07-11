linode-cli lke cluster-create \
  --label my-linode-cluster \
  --region eu-central \
  --k8s_version 1.31 \
  --node_pools.type g6-standard-2 \
  --node_pools.count 2

http://139.162.128.229:31869/

  sgworker@MacBook-Pro-3.local /Users/sgworker/Desktop/CAPSTONE-PROJECT-4 
% linode-cli lke clusters-list
┌────────┬───────────────────┬────────────┬─────────────┬─────────────────────────────────┬──────┐
│ id     │ label             │ region     │ k8s_version │ control_plane.high_availability │ tier │
├────────┼───────────────────┼────────────┼─────────────┼─────────────────────────────────┼──────┤
│ 497779 │ my-linode-cluster │ eu-central │ 1.31        │ False                           │      │
└────────┴───────────────────┴────────────┴─────────────┴─────────────────────────────────┴──────┘
sgworker@MacBook-Pro-3.local /Users/sgworker/Desktop/CAPSTONE-PROJECT-4 
% linode-cli lke cluster-delete 497779
sgworker@MacBook-Pro-3.local /Users/sgworker/Desktop/CAPSTONE-PROJECT-4 
% linode-cli lke clusters-list        
┌────┬───────┬────────┬─────────────┬─────────────────────────────────┬──────┐
│ id │ label │ region │ k8s_version │ control_plane.high_availability │ tier │
├────┼───────┼────────┼─────────────┼─────────────────────────────────┼──────┤
└────┴───────┴────────┴─────────────┴─────────────────────────────────┴──────┘
sgworker@MacBook-Pro-3.local /Users/sgworker/Desktop/CAPSTONE-PROJECT-4 
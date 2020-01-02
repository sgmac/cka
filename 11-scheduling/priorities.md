# Priorities

- You can assign pods via the `PriorityClassName`, this allows users to preemt, or evict lower priority pods. f a node is found, the low priority pod(s) are evicted and the higher priority pod is scheduled. The use of a Pod Disruption Budget (PDB) is a way to limit the number of pods preemption evicts to ensure enough pods remain running. The scheduler will remove pods even if the PDB is violated if no other options are available.



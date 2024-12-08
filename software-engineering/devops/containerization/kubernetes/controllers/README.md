# Controllers

Orchestration is managed through a series of watch-loops, also known controllers (or operators). Each controller asks the kube-apiserver for a particular object state, modifying the object until the declared state matches the current state.

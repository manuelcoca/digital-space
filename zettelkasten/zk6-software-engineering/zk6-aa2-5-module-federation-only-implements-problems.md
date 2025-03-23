**`id`**: zk6-aa-2
**`title`**: 5-module-federation-only-implements-problems
**`date`**: 2025-03-13
**`tags`**: #micro-frontends #frontend-architecture #module-federation #dependencies #peerDependencies #devDependencies

---

###### Content

Micro-frontends with module federation (or other tooling) adds more problems than it solves. The only advantage is sharing packages, which is a very tiny size advantage for react. Using different react versions between micro-frontends already adds new problems and will not work without more effort. That debunks the most important factor: Architecture that allows gradual breaking changes.

###### References

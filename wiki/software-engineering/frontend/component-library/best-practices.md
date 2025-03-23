### Monopackage vs. modularization

#frontend-architecture #frontend-tooling #component-library #packagejson #micro-frontends

#### Monopackage

+Less maintenance for consumers when upgrading (avoids issues with peer dependencies) +Less issues with version conflicts +Better DX and efficiency +Less maintenance

#### Modularization

+Smaller releases and better release strategy of breaking changes in single components +Independent package versioning +Faster time to market +Smaller bundles (if bundler does tree shaking, the build should be optimised)

#### Adobe react spectrum

Component Versioning documentation: https://github.com/adobe/react-spectrum/blob/main/rfcs/2019-v3-versioning.md React spectrum discussions: https://github.com/adobe/react-spectrum/discussions/6734

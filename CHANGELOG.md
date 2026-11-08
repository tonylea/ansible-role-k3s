# Changelog

All notable changes to this project will be documented in this file. See [commit-and-tag-version](https://github.com/absolute-version/commit-and-tag-version) for commit guidelines.

## 1.0.0 (2026-11-08)

### Features

* add extra names, such as a DNS alias, to the API certificate ([36207f9](https://github.com/tonylea/ansible-role-k3s/commit/36207f9d9219d49eb48ac5665e7f5915c0721d13))
* install a pinned k3s server or agent, with its config and firewall rules ([87c6fd4](https://github.com/tonylea/ansible-role-k3s/commit/87c6fd4c82aa15b0d9b2a641e11054dfbead080a))
* install iptables and conntrack, which k3s uses from the host ([5fd9505](https://github.com/tonylea/ansible-role-k3s/commit/5fd9505c889274e54f7725e84edb0e2c7240dc6f))
* label agents as workers so kubectl shows their role ([e667f84](https://github.com/tonylea/ansible-role-k3s/commit/e667f84c409fae0b506f69a7b6d290457146948a))
* taint the server so only workloads that tolerate it run there ([2325477](https://github.com/tonylea/ansible-role-k3s/commit/23254779eb8dd617f1dbc8e581b166b0bd5d88d0))
* wait for the Kubernetes API, and for each agent to join, before moving on ([0bebbbd](https://github.com/tonylea/ansible-role-k3s/commit/0bebbbd7f21bc2d3d3a161f7bc3e6a78db1e686f))

### Bug Fixes

* append the cgroup parameters in the same order every time ([002435b](https://github.com/tonylea/ansible-role-k3s/commit/002435b31e8d042f6e6fce126c8779b6ea3a5ca0))
* keep the cgroup parameters on the kernel command line's one line ([379848d](https://github.com/tonylea/ansible-role-k3s/commit/379848d7ce1a04789dd0406536b74160b9feeaa0))
* only delegate reading the join token on agents ([b86445f](https://github.com/tonylea/ansible-role-k3s/commit/b86445f60a519ecaa147dbe038d1b58783b5a9b7))
* point the saved kubeconfig at the server, not the machine running Ansible ([057e511](https://github.com/tonylea/ansible-role-k3s/commit/057e5110965b75fbd2eb3f44f94bcfe9d1d33149))
* read the agent's labels as JSON so the worker label is only set once ([4f6623d](https://github.com/tonylea/ansible-role-k3s/commit/4f6623db824b6dafb462bb810c842f1a8caf54b0))

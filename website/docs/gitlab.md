---
sidebar_position: 10
---

# GitLab CI/CD

It is possible to generate changelogs using [GitLab CI/CD](https://docs.gitlab.com/ee/ci/).

This minimal example creates artifacts that can be used on another job.

```yml
- changelog:
    image:
      name: orhunp/git-cliff:latest
      entrypoint: [""]
    variables:
      GIT_STRATEGY: clone # clone entire repo instead of reusing workspace
      GIT_DEPTH: 0 # avoid shallow clone to give cliff all the info it needs
    stage: doc
    script:
      - git-cliff -r . > CHANGELOG.md
    artifacts:
      paths:
        - CHANGELOG.md
```

Please note that the stage is `doc` and has to be changed accordingly to your need.

:::info

If you are using a GitLab self-managed instance with custom certificates, you can use this alternative [GitLab CI/CD](https://docs.gitlab.com/ee/ci/).

```yml
changelog:
  image:
    name: orhunp/git-cliff:latest
    entrypoint: [""]
  variables:
    GIT_STRATEGY: clone # clone entire repo instead of reusing workspace
    GIT_DEPTH: 0 # avoid shallow clone to give cliff all the info it needs
  stage: doc
  before_script:
    - mkdir -p usr/local/share/ca-certificates/
    - cp "$CI_SERVER_TLS_CA_FILE" /usr/local/share/ca-certificates/gitlab-ca.crt
    - update-ca-certificates
  script:
    - git-cliff --use-native-tls -r . > CHANGELOG.md
  artifacts:
    paths:
      - CHANGELOG.md
```

:::

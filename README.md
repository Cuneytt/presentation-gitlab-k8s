我是光年实验室高级招聘经理。
我在github上访问了你的开源项目，你的代码超赞。你最近有没有在看工作机会，我们在招软件开发工程师，拉钩和BOSS等招聘网站也发布了相关岗位，有公司和职位的详细信息。
我们公司在杭州，业务主要做流量增长，是很多大型互联网公司的流量顾问。公司弹性工作制，福利齐全，发展潜力大，良好的办公环境和学习氛围。
公司官网是http://www.gnlab.com,公司地址是杭州市西湖区古墩路紫金广场B座，若你感兴趣，欢迎与我联系，
电话是0571-88839161，手机号：18668131388，微信号：echo 'bGhsaGxoMTEyNAo='|base64 -D ,静待佳音。如有打扰，还请见谅，祝生活愉快工作顺利。

# presentation-gitlab-k8s

These are the example files for my presentation about GitLab + Kubernetes for Continuous Integration and Delivery. They are also partly used in my GitLab CI posts.

![Kubernetes and GitLab](/media/kubernetes-and-gitLab.png)

**INFO** This isn't the best way to deploy application Docker images to K8s, this is more of an example how simple it can be.

The presentation can be found here: [Kubernetes - WYNTK - GitLab CI + Kubernetes Presentation](https://edenmal.moe/post/2017/Kubernetes-WYNTK-GitLab-CI-Kubernetes-Presentation/).
The blog post these files are specifically used in is here: [GitLab + Kubernetes: Perfect Match for Continuous Delivery with Container](https://edenmal.moe/post/2017/GitLab-Kubernetes-Perfect-Match-for-Continuous-Delivery-with-Container/).

An uptodate list of all my blog posts around GitLab and Kubernetes can be found on [this page](https://edenmal.moe/tags/gitlab/).
This list is just an excerpt of some of my GitLab posts:

* [GitLab + Kubernetes: Using GitLab CI's Kubernetes Cluster feature](https://edenmal.moe/post/2018/GitLab-Kubernetes-Using-GitLab-CIs-Kubernetes-Cluster-feature/)
* [GitLab + Kubernetes: Perfect Match for Continuous Delivery with Container](https://edenmal.moe/post/2017/GitLab-Kubernetes-Perfect-Match-for-Continuous-Delivery-with-Container/)
* [Kubernetes - WYNTK - GitLab CI + Kubernetes Presentation](https://edenmal.moe/post/2017/Kubernetes-WYNTK-GitLab-CI-Kubernetes-Presentation/)
* [GitLab + Kubernetes: Running CI Runners in Kubernetes](https://edenmal.moe/post/2017/GitLab-Kubernetes-Running-CI-Runners-in-Kubernetes/)
* [GitLab + Kubernetes: GitLab on top of Kubernetes](https://edenmal.moe/post/2017/GitLab-Kubernetes-GitLab-on-top-of-Kubernetes/)
* [GitLab: Use Keycloak as SAML 2.0 OmniAuth Provider](https://edenmal.moe/post/2018/GitLab-Keycloak-SAML-2-0-OmniAuth-Provider/)

## Table of Contents

* [Requirements](#requirements)
* [Features](#features)
* [Using this repository](#using-this-repository)
* [GitLab Docs References](#gitlab-docs-references)
* [File Structure](#file-structure)
    * [Example Application](#example-application)
    * [Kubernetes Base GitLab CI Manifests](#kubernetes-base-gitlab-ci-manifests)
    * [Build Process](#build-process)
    * [Deployment Manifests](#deployment-manifests)
    * [Miscellaneous](#miscellaneous)
* [Thanks!](#thanks)

## Features

This repository shows off/uses the following GitLab CI features:
* [GitLab CI](https://docs.gitlab.com/ce/ci/README.html)
    * [Manual CI Steps](https://docs.gitlab.com/ce/ci/yaml/#when-manual)
    * [Artifacts](https://docs.gitlab.com/ce/user/project/pipelines/job_artifacts.html)
    * [App review](https://docs.gitlab.com/ce/ci/review_apps/index.html)
* [GitLab Container Registry](https://docs.gitlab.com/ce/user/project/container_registry.html)
* [GitLab CI Kubernetes Cluster Integration](https://docs.gitlab.com/ce/user/project/clusters/index.html)

Other features also shown are:
* [coreos/prometheus-operator ServiceMonitor]() - for automatic monitoring of deployed applications.

## Requirements

The following points are required for this repository to work correctly:
* GitLab (`>= 11.3`) with the following features configured:
    * [Container Registry](https://docs.gitlab.com/ce/user/project/container_registry.html)
    * [GitLab CI](https://about.gitlab.com/features/gitlab-ci-cd/) (with working [GitLab CI Runners](https://docs.gitlab.com/ce/ci/runners/), at least version `>= 11.3`)
* [Kubernetes](https://kubernetes.io/) cluster
    * You need to be "bound" to the `admin` (`cluster-admin`) ClusterRole, see [Kubernetes.io Using RBAC Authorization - User-facing Roles](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#user-facing-roles).
    * An Ingress controller should already been deployed, see [Kubernetes.io Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/).
* `kubectl` installed locally.
* Editor of your choice.

## Using this repository

You have to replace the following addresses in all files:
* `gitlab.zerbytes.net` with your GitLab address (e.g. `gitlab.example.com`).
* `edenmal.net` (in the Ingress manifest) with your domain name.
    * You probably also want to change the subdomain name while you are at it.
* `presentation-gitlab-k8s` with the Namespace name of your choice.

If you are using [coreos/prometheus-operator](https://github.com/coreos/prometheus-operator), then you also need to replace
`zerbytes-live-proj-monitoring` with the Namespace your Prometheus instance is running in,
in this file [`/gitlab-ci/monitoring/service-monitor.yaml`](/gitlab-ci/monitoring/service-monitor.yaml).
You then also want to `kubectl` create/apply the file to your Kubernetes cluster during creation/apply process for the manifests in [`gitlab-ci/`](/gitlab-ci/).

You also need to create a "Docker Login" Secret which contains your GitLab Registry access data (e.g. Username and Access token with registry access) named `regsecret` in the Namespace `presentation-gitlab-k8s`.
A guide for that can be found here: [Kubernetes.io - Pull an Image from a Private Registry](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/).
The Namespace manifest is in the [`gitlab-ci/`](/gitlab-ci/) directory.

Then you can just import the repository into your GitLab instance and are ready to go.

For information on how to use these files and setup GitLab Kubernetes cluster/integration, see the above blog post and in specific this post [GitLab + Kubernetes: Perfect Match for Continuous Delivery with Container](https://edenmal.moe/post/2017/GitLab-Kubernetes-Perfect-Match-for-Continuous-Delivery-with-Container/).

## GitLab Docs References

* GitLab Kubernetes Integration Docs: https://docs.gitlab.com/ce/user/project/integrations/kubernetes.html
* GitLab Kubernetes Integration Docs Environment variables: https://docs.gitlab.com/ce/user/project/integrations/kubernetes.html#deployment-variables

As of GitLab `10.3` the Kubernetes Integration is marked as deprecated and with `10.4` it is now disabled, the following docs show the new feature called Clusters:
* GitLab 10.3 release - Kubernetes integration service: https://about.gitlab.com/2017/12/22/gitlab-10-3-released/#kubernetes-integration-service
* GitLab Clusters Feature Docs: https://docs.gitlab.com/ce/user/project/clusters/index.html

## File Structure

### Example Application

* [`main.go`](/main.go) - The Golang example application code.
* [`vendor/`](/vendor/) - Contains the Golang example application dependencies (`dep` is used).
* [`Gopkg.lock`](`/Gopkg.lock`) and [`Gopkg.toml`](`/Gopkg.toml`) - Golang `dep` .

### Kubernetes Base GitLab CI Manifests

* [`gitlab-ci/`](/gitlab-ci/)
    * [`monitoring/`](/gitlab-ci/monitoring/)
        * [`service-monitor.yaml`](/gitlab-ci/monitoring/service-monitor.yaml) - Contains a coreos/prometheus-operator ServiceMonitor manifest to automatically monitor the application(s).
    * [`namespace.yaml`](/gitlab-ci/namespace.yaml) - Namespace in which the GitLab CI will deploy the application.
    * [`rbac.yaml`](/gitlab-ci/rbac.yaml) - Contains GitLab CI RBAC Role, RoleBinding and ServiceAccount.
    * [`secret.yaml`](/gitlab-ci/secret.yaml) - Contains a TLS wildcard certificate for the application Ingress.

### Build Process

* [`Dockerfile`](/Dockerfile) - Contains the Docker image build instructions.
* [`.gitlab-ci.yml`](/.gitlab-ci.yml) - Contains the GitLab CI instructions.

### Deployment Manifests

* [`manifests/`](/manifests/) - Kubernetes manifests used to deploy the Docker image built in the CI pipeline.
    * [`deployment.yaml`](/manifests/deployment.yaml) - Deployment for the Docker image.
    * [`ingress.yaml`](/manifests/ingress.yaml) - Ingress for the application.
    * [`service.yaml`](/manifests/service.yaml) - Service for the application.

### Miscellaneous

* [`media/`](/media/) - Contains media for the [`README.md`](/README.md) in this repository.

## Thanks!

Thanks to [@shadycuz - GitHub](https://github.com/shadycuz) for his comments with improvements for the code in this repository!

## License

The files in this repo can be used under the MIT license, see [LICENSE](/LICENSE) file.

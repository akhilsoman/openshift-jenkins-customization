Preface
=======

This repository is used for customize Jenkins for daily DevOps tasks needed.


Customization
=============

## Plugins

Please refer to the [source code](plugins.txt) for detail additionally installed plugin list.

## Script Approvals

Please refer to the [source code](configuration/scriptApproval.xml) for pre-approved script list.


Usage
=====

## 1. Build new Jenkins image

Before executing commands below, you need to use 'oc' to log into OpenShift platform at first.

```bash
# Get source code
$ git clone https://github.com/godleon/learning_openshift.git

# Create Jenkins Image Stream & Build for rebuilding Jenkins Image
$ oc create -f openshift-jenkins-customization/openshift-objects/

# Start build job
$ oc -n openshift start-build custom-jenkins-build

# Check pod status of build job
$ oc -n openshift get pods
NAME                           READY     STATUS      RESTARTS   AGE
custom-jenkins-build-1-build   0/1       Completed   0          22m

# Check build log
$ oc -n openshift logs -f custom-jenkins-build-1-build
```


## 2. Modify Jenkins template

Modify `jenkins-ephemeral` template in **openshift** namespace for global usage: 

```bash
$ oc edit template/jenkins-ephemeral -n openshift
```

Then, change the `JENKINS_IMAGE_STREAM_TAG` in **parameters** section from `jenkins:latest` to `custom-jenkins-2-centos7:latest`.

You are good to go now.


References
==========

- [openshift/jenkins](https://github.com/openshift/jenkins/)

- [Script approvals needed for changes to build config Jenkinsfile · Issue #57 · openshift/jenkins-sync-plugin](https://github.com/openshift/jenkins-sync-plugin/issues/57)
# Kimlic Charts

Helm uses a packaging format called _charts_. A chart is a collection of files
that describe a related set of Kubernetes resources. A single chart
might be used to deploy something simple, like a memcached pod, or
something complex, like a full web app stack with HTTP servers,
databases, caches, and so on.

Charts are created as files laid out in a particular directory tree,
then they can be packaged into versioned archives to be deployed.

This document explains the chart format, and provides basic guidance for
building charts with Helm.

## The Chart File Structure

A chart is organized as a collection of files inside of a directory. The
directory name is the name of the chart (without versioning information). Thus,
a chart describing WordPress would be stored in the `wordpress/` directory.

Inside of this directory, Helm will expect a structure that matches this:

```
wordpress/
  Chart.yaml          # A YAML file containing information about the chart
  LICENSE             # OPTIONAL: A plain text file containing the license for the chart
  README.md           # OPTIONAL: A human-readable README file
  values.yaml         # The default configuration values for this chart
  charts/             # OPTIONAL: A directory containing any charts upon which this chart depends.
  templates/          # OPTIONAL: A directory of templates that, when combined with values,
                      # will generate valid Kubernetes manifest files.
  templates/NOTES.txt # OPTIONAL: A plain text file containing short usage notes
```

Helm reserves use of the `charts/` and `templates/` directories, and of
the listed file names. Other files will be left as they are.

## Installing Helm

On macOS, run:

```
brew install kubernetes-helm
```

## How to deploy?

### Listing all releases

Use `helm ls` command to list all releases.

Example:

```
$ helm ls
NAME            REVISION        UPDATED                         STATUS          CHART                   NAMESPACE  
logging         3               Mon Jul 23 13:09:44 2018        FAILED          logging-0.1.1           logging    
master-dev      6               Tue Aug  7 18:21:16 2018        DEPLOYED        quorum-0.1.1            dev        
mobile-api      27              Wed Aug  8 14:40:58 2018        DEPLOYED        mobile-api-0.1.1        dev        
mobile-api-test 20              Wed Aug  8 19:04:09 2018        DEPLOYED        mobile-api-0.1.1        test       
node-slave1     2               Thu Aug  2 20:12:18 2018        DEPLOYED        quorum-0.1.1            test       
node-slave2     3               Tue Aug  7 21:59:39 2018        DEPLOYED        quorum-0.1.1            test       
node1-test      3               Thu Aug  2 20:17:26 2018        DEPLOYED        quorum-0.1.1            test       
rabbitmq        1               Wed Jul  4 18:30:09 2018        DEPLOYED        rabbitmq-1.1.3          dev        
rabbitmq-test   1               Sun Jul 29 19:26:25 2018        DEPLOYED        rabbitmq-1.1.3          test       
traefik         5               Mon Jul 23 16:11:06 2018        DEPLOYED        traefik-1.34.0          kube-system
```

### Deploying a new version of a service

Set new value for `tag` in `values-%ENV%.yaml`.
Tag value it's a Docker container tag, that located in docker hub for [mobile_api](https://hub.docker.com/r/kimlictr/mobile_api/tags/) or [attestation_api](https://hub.docker.com/r/kimlictr/attestation_api/tags/). 

Then run the following command:

```
$ helm upgrade -f [values_file] [NAME] [chart_dir]
```

Example:

```
# for dev
$ helm upgrade -f mobile-api/values-dev.yaml mobile-api mobile-api

# for test
$ helm upgrade -f mobile-api/values-test.yaml mobile-api-test mobile-api
```

### Roll back a release to a previous revision

Use the following command to rollback a release:

```
$ helm rollback [RELEASE] [REVISION]
```

Example:

```
helm rollback mobile-api 6
```

---
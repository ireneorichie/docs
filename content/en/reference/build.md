---
title: "Knative Build Component"
linkTitle: "Build API"
weight: 20
type: "docs"
---
<h1>API Reference Documentation</h1>
<h2>
build.knative.dev
</h2>
<p>
Package v1alpha1 is the v1alpha1 version of the API.
</p>
Resource Types:
<ul><li>
<a href="#Build">Build</a>
</li><li>
<a href="#BuildTemplate">BuildTemplate</a>
</li><li>
<a href="#ClusterBuildTemplate">ClusterBuildTemplate</a>
</li></ul>
<h3 id="Build">Build
</h3>
<p>
Build represents a build of a container image. A Build is made up of a
source, and a set of steps. Steps can mount volumes to share data between
themselves. A build may be created by instantiating a BuildTemplate.
</p>
<table>
<thead>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>apiVersion</code></br>
string</td>
<td>
<code>
build.knative.dev/v1alpha1
</code>
</td>
</tr>
<tr>
<td>
<code>kind</code></br>
string
</td>
<td><code>Build</code></td>
</tr>
<tr>
<td>
<code>metadata</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectmeta-v1-meta">
Kubernetes meta/v1.ObjectMeta
</a>
</em>
</td>
<td>
Refer to the Kubernetes API documentation for the fields of the
<code>metadata</code> field.
</td>
</tr>
<tr>
<td>
<code>spec</code></br>
<em>
<a href="#BuildSpec">
BuildSpec
</a>
</em>
</td>
<td>
<br/>
<br/>
<table>
<tr>
<td>
<code>generation</code></br>
<em>
int64
</em>
</td>
<td>
<em>(Optional)</em>
TODO: Generation does not work correctly with CRD. They are scrubbed
by the APIserver (https://github.com/kubernetes/kubernetes/issues/58778)
So, we add Generation here. Once that gets fixed, remove this and use
ObjectMeta.Generation instead.
</td>
</tr>
<tr>
<td>
<code>source</code></br>
<em>
<a href="#SourceSpec">
SourceSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Source specifies the input to the build.
</td>
</tr>
<tr>
<td>
<code>sources</code></br>
<em>
<a href="#SourceSpec">
[]SourceSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Sources specifies the inputs to the build.
</td>
</tr>
<tr>
<td>
<code>steps</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#container-v1-core">
[]Kubernetes core/v1.Container
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Steps are the steps of the build; each step is run sequentially with the
source mounted into /workspace.
</td>
</tr>
<tr>
<td>
<code>volumes</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#volume-v1-core">
[]Kubernetes core/v1.Volume
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Volumes is a collection of volumes that are available to mount into the
steps of the build.
</td>
</tr>
<tr>
<td>
<code>serviceAccountName</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
The name of the service account as which to run this build.
</td>
</tr>
<tr>
<td>
<code>template</code></br>
<em>
<a href="#TemplateInstantiationSpec">
TemplateInstantiationSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Template, if specified, references a BuildTemplate resource to use to
populate fields in the build, and optional Arguments to pass to the
template. The default Kind of template is BuildTemplate
</td>
</tr>
<tr>
<td>
<code>nodeSelector</code></br>
<em>
map[string]string
</em>
</td>
<td>
<em>(Optional)</em>
NodeSelector is a selector which must be true for the pod to fit on a node.
Selector which must match a node's labels for the pod to be scheduled on that node.
More info: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
</td>
</tr>
<tr>
<td>
<code>timeout</code></br>
<em>
<a href="https://godoc.org/k8s.io/apimachinery/pkg/apis/meta/v1#Duration">
Kubernetes meta/v1.Duration
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Time after which the build times out. Defaults to 10 minutes.
Specified build timeout should be less than 24h.
Refer Go's ParseDuration documentation for expected format: https://golang.org/pkg/time/#ParseDuration
</td>
</tr>
<tr>
<td>
<code>affinity</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#affinity-v1-core">
Kubernetes core/v1.Affinity
</a>
</em>
</td>
<td>
<em>(Optional)</em>
If specified, the pod's scheduling constraints
</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>
<code>status</code></br>
<em>
<a href="#BuildStatus">
BuildStatus
</a>
</em>
</td>
<td>
</td>
</tr>
</tbody>
</table>
<h3 id="BuildTemplate">BuildTemplate
</h3>
<p>
BuildTemplate is a template that can used to easily create Builds.
</p>
<table>
<thead>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>apiVersion</code></br>
string</td>
<td>
<code>
build.knative.dev/v1alpha1
</code>
</td>
</tr>
<tr>
<td>
<code>kind</code></br>
string
</td>
<td><code>BuildTemplate</code></td>
</tr>
<tr>
<td>
<code>metadata</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectmeta-v1-meta">
Kubernetes meta/v1.ObjectMeta
</a>
</em>
</td>
<td>
Refer to the Kubernetes API documentation for the fields of the
<code>metadata</code> field.
</td>
</tr>
<tr>
<td>
<code>spec</code></br>
<em>
<a href="#BuildTemplateSpec">
BuildTemplateSpec
</a>
</em>
</td>
<td>
<br/>
<br/>
<table>
<tr>
<td>
<code>generation</code></br>
<em>
int64
</em>
</td>
<td>
<em>(Optional)</em>
TODO: Generation does not work correctly with CRD. They are scrubbed
by the APIserver (https://github.com/kubernetes/kubernetes/issues/58778)
So, we add Generation here. Once that gets fixed, remove this and use
ObjectMeta.Generation instead.
</td>
</tr>
<tr>
<td>
<code>parameters</code></br>
<em>
<a href="#ParameterSpec">
[]ParameterSpec
</a>
</em>
</td>
<td>
Parameters defines the parameters that can be populated in a template.
</td>
</tr>
<tr>
<td>
<code>steps</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#container-v1-core">
[]Kubernetes core/v1.Container
</a>
</em>
</td>
<td>
Steps are the steps of the build; each step is run sequentially with the
source mounted into /workspace.
</td>
</tr>
<tr>
<td>
<code>volumes</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#volume-v1-core">
[]Kubernetes core/v1.Volume
</a>
</em>
</td>
<td>
Volumes is a collection of volumes that are available to mount into the
steps of the build.
</td>
</tr>
</table>
</td>
</tr>
</tbody>
</table>
<h3 id="ClusterBuildTemplate">ClusterBuildTemplate
</h3>
<p>
ClusterBuildTemplate is a template that can used to easily create Builds.
</p>
<table>
<thead>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>apiVersion</code></br>
string</td>
<td>
<code>
build.knative.dev/v1alpha1
</code>
</td>
</tr>
<tr>
<td>
<code>kind</code></br>
string
</td>
<td><code>ClusterBuildTemplate</code></td>
</tr>
<tr>
<td>
<code>metadata</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectmeta-v1-meta">
Kubernetes meta/v1.ObjectMeta
</a>
</em>
</td>
<td>
Refer to the Kubernetes API documentation for the fields of the
<code>metadata</code> field.
</td>
</tr>
<tr>
<td>
<code>spec</code></br>
<em>
<a href="#BuildTemplateSpec">
BuildTemplateSpec
</a>
</em>
</td>
<td>
<br/>
<br/>
<table>
<tr>
<td>
<code>generation</code></br>
<em>
int64
</em>
</td>
<td>
<em>(Optional)</em>
TODO: Generation does not work correctly with CRD. They are scrubbed
by the APIserver (https://github.com/kubernetes/kubernetes/issues/58778)
So, we add Generation here. Once that gets fixed, remove this and use
ObjectMeta.Generation instead.
</td>
</tr>
<tr>
<td>
<code>parameters</code></br>
<em>
<a href="#ParameterSpec">
[]ParameterSpec
</a>
</em>
</td>
<td>
Parameters defines the parameters that can be populated in a template.
</td>
</tr>
<tr>
<td>
<code>steps</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#container-v1-core">
[]Kubernetes core/v1.Container
</a>
</em>
</td>
<td>
Steps are the steps of the build; each step is run sequentially with the
source mounted into /workspace.
</td>
</tr>
<tr>
<td>
<code>volumes</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#volume-v1-core">
[]Kubernetes core/v1.Volume
</a>
</em>
</td>
<td>
Volumes is a collection of volumes that are available to mount into the
steps of the build.
</td>
</tr>
</table>
</td>
</tr>
</tbody>
</table>
<h3 id="ArgumentSpec">ArgumentSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#TemplateInstantiationSpec">TemplateInstantiationSpec</a>)
</p>
<p>
ArgumentSpec defines the actual values to use to populate a template's
parameters.
</p>
<table>
<thead>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>name</code></br>
<em>
string
</em>
</td>
<td>
Name is the name of the argument.
</td>
</tr>
<tr>
<td>
<code>value</code></br>
<em>
string
</em>
</td>
<td>
Value is the value of the argument.
</td>
</tr>
</tbody>
</table>
<h3 id="BuildProvider">BuildProvider
(<code>string</code> alias)</p></h3>
<p>
(<em>Appears on:</em>
<a href="#BuildStatus">BuildStatus</a>)
</p>
<p>
BuildProvider defines a build execution implementation.
</p>
<h3 id="BuildSpec">BuildSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#Build">Build</a>)
</p>
<p>
BuildSpec is the spec for a Build resource.
</p>
<table>
<thead>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>generation</code></br>
<em>
int64
</em>
</td>
<td>
<em>(Optional)</em>
TODO: Generation does not work correctly with CRD. They are scrubbed
by the APIserver (https://github.com/kubernetes/kubernetes/issues/58778)
So, we add Generation here. Once that gets fixed, remove this and use
ObjectMeta.Generation instead.
</td>
</tr>
<tr>
<td>
<code>source</code></br>
<em>
<a href="#SourceSpec">
SourceSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Source specifies the input to the build.
</td>
</tr>
<tr>
<td>
<code>sources</code></br>
<em>
<a href="#SourceSpec">
[]SourceSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Sources specifies the inputs to the build.
</td>
</tr>
<tr>
<td>
<code>steps</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#container-v1-core">
[]Kubernetes core/v1.Container
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Steps are the steps of the build; each step is run sequentially with the
source mounted into /workspace.
</td>
</tr>
<tr>
<td>
<code>volumes</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#volume-v1-core">
[]Kubernetes core/v1.Volume
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Volumes is a collection of volumes that are available to mount into the
steps of the build.
</td>
</tr>
<tr>
<td>
<code>serviceAccountName</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
The name of the service account as which to run this build.
</td>
</tr>
<tr>
<td>
<code>template</code></br>
<em>
<a href="#TemplateInstantiationSpec">
TemplateInstantiationSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Template, if specified, references a BuildTemplate resource to use to
populate fields in the build, and optional Arguments to pass to the
template. The default Kind of template is BuildTemplate
</td>
</tr>
<tr>
<td>
<code>nodeSelector</code></br>
<em>
map[string]string
</em>
</td>
<td>
<em>(Optional)</em>
NodeSelector is a selector which must be true for the pod to fit on a node.
Selector which must match a node's labels for the pod to be scheduled on that node.
More info: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
</td>
</tr>
<tr>
<td>
<code>timeout</code></br>
<em>
<a href="https://godoc.org/k8s.io/apimachinery/pkg/apis/meta/v1#Duration">
Kubernetes meta/v1.Duration
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Time after which the build times out. Defaults to 10 minutes.
Specified build timeout should be less than 24h.
Refer Go's ParseDuration documentation for expected format: https://golang.org/pkg/time/#ParseDuration
</td>
</tr>
<tr>
<td>
<code>affinity</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#affinity-v1-core">
Kubernetes core/v1.Affinity
</a>
</em>
</td>
<td>
<em>(Optional)</em>
If specified, the pod's scheduling constraints
</td>
</tr>
</tbody>
</table>
<h3 id="BuildStatus">BuildStatus
</h3>
<p>
(<em>Appears on:</em>
<a href="#Build">Build</a>)
</p>
<p>
BuildStatus is the status for a Build resource
</p>
<table>
<thead>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>builder</code></br>
<em>
<a href="#BuildProvider">
BuildProvider
</a>
</em>
</td>
<td>
<em>(Optional)</em>
</td>
</tr>
<tr>
<td>
<code>cluster</code></br>
<em>
<a href="#ClusterSpec">
ClusterSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Cluster provides additional information if the builder is Cluster.
</td>
</tr>
<tr>
<td>
<code>google</code></br>
<em>
<a href="#GoogleSpec">
GoogleSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Google provides additional information if the builder is Google.
</td>
</tr>
<tr>
<td>
<code>startTime</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#time-v1-meta">
Kubernetes meta/v1.Time
</a>
</em>
</td>
<td>
<em>(Optional)</em>
StartTime is the time the build is actually started.
</td>
</tr>
<tr>
<td>
<code>completionTime</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#time-v1-meta">
Kubernetes meta/v1.Time
</a>
</em>
</td>
<td>
<em>(Optional)</em>
CompletionTime is the time the build completed.
</td>
</tr>
<tr>
<td>
<code>stepStates</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#containerstate-v1-core">
[]Kubernetes core/v1.ContainerState
</a>
</em>
</td>
<td>
<em>(Optional)</em>
StepStates describes the state of each build step container.
</td>
</tr>
<tr>
<td>
<code>stepsCompleted</code></br>
<em>
[]string
</em>
</td>
<td>
<em>(Optional)</em>
StepsCompleted lists the name of build steps completed.
</td>
</tr>
<tr>
<td>
<code>conditions</code></br>
<em>
<a href="https://godoc.org/github.com/knative/pkg/apis/duck/v1alpha1#Conditions">
github.com/knative/pkg/apis/duck/v1alpha1.Conditions
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Conditions describes the set of conditions of this build.
</td>
</tr>
</tbody>
</table>
<h3 id="BuildTemplateInterface">BuildTemplateInterface
</h3>
<p>
BuildTemplateInterface is implemented by BuildTemplate and ClusterBuildTemplate
</p>
<h3 id="BuildTemplateSpec">BuildTemplateSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#BuildTemplate">BuildTemplate</a>,
<a href="#ClusterBuildTemplate">ClusterBuildTemplate</a>)
</p>
<p>
BuildTemplateSpec is the spec for a BuildTemplate.
</p>
<table>
<thead>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>generation</code></br>
<em>
int64
</em>
</td>
<td>
<em>(Optional)</em>
TODO: Generation does not work correctly with CRD. They are scrubbed
by the APIserver (https://github.com/kubernetes/kubernetes/issues/58778)
So, we add Generation here. Once that gets fixed, remove this and use
ObjectMeta.Generation instead.
</td>
</tr>
<tr>
<td>
<code>parameters</code></br>
<em>
<a href="#ParameterSpec">
[]ParameterSpec
</a>
</em>
</td>
<td>
Parameters defines the parameters that can be populated in a template.
</td>
</tr>
<tr>
<td>
<code>steps</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#container-v1-core">
[]Kubernetes core/v1.Container
</a>
</em>
</td>
<td>
Steps are the steps of the build; each step is run sequentially with the
source mounted into /workspace.
</td>
</tr>
<tr>
<td>
<code>volumes</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#volume-v1-core">
[]Kubernetes core/v1.Volume
</a>
</em>
</td>
<td>
Volumes is a collection of volumes that are available to mount into the
steps of the build.
</td>
</tr>
</tbody>
</table>
<h3 id="ClusterSpec">ClusterSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#BuildStatus">BuildStatus</a>)
</p>
<p>
ClusterSpec provides information about the on-cluster build, if applicable.
</p>
<table>
<thead>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>namespace</code></br>
<em>
string
</em>
</td>
<td>
Namespace is the namespace in which the pod is running.
</td>
</tr>
<tr>
<td>
<code>podName</code></br>
<em>
string
</em>
</td>
<td>
PodName is the name of the pod responsible for executing this build's steps.
</td>
</tr>
</tbody>
</table>
<h3 id="GCSSourceSpec">GCSSourceSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#SourceSpec">SourceSpec</a>)
</p>
<p>
GCSSourceSpec describes source input to the Build in the form of an archive,
or a source manifest describing files to fetch.
</p>
<table>
<thead>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>type</code></br>
<em>
<a href="#GCSSourceType">
GCSSourceType
</a>
</em>
</td>
<td>
Type declares the style of source to fetch.
</td>
</tr>
<tr>
<td>
<code>location</code></br>
<em>
string
</em>
</td>
<td>
Location specifies the location of the source archive or manifest file.
</td>
</tr>
</tbody>
</table>
<h3 id="GCSSourceType">GCSSourceType
(<code>string</code> alias)</p></h3>
<p>
(<em>Appears on:</em>
<a href="#GCSSourceSpec">GCSSourceSpec</a>)
</p>
<p>
GCSSourceType defines a type of GCS source fetch.
</p>
<h3 id="GitSourceSpec">GitSourceSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#SourceSpec">SourceSpec</a>)
</p>
<p>
GitSourceSpec describes a Git repo source input to the Build.
</p>
<table>
<thead>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>url</code></br>
<em>
string
</em>
</td>
<td>
URL of the Git repository to clone from.
</td>
</tr>
<tr>
<td>
<code>revision</code></br>
<em>
string
</em>
</td>
<td>
Git revision (branch, tag, commit SHA or ref) to clone.  See
https://git-scm.com/docs/gitrevisions#_specifying_revisions for more
information.
</td>
</tr>
</tbody>
</table>
<h3 id="GoogleSpec">GoogleSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#BuildStatus">BuildStatus</a>)
</p>
<p>
GoogleSpec provides information about the GCB build, if applicable.
</p>
<table>
<thead>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>operation</code></br>
<em>
string
</em>
</td>
<td>
Operation is the unique name of the GCB API Operation for the build.
</td>
</tr>
</tbody>
</table>
<h3 id="ParameterSpec">ParameterSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#BuildTemplateSpec">BuildTemplateSpec</a>)
</p>
<p>
ParameterSpec defines the possible parameters that can be populated in a
template.
</p>
<table>
<thead>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>name</code></br>
<em>
string
</em>
</td>
<td>
Name is the unique name of this template parameter.
</td>
</tr>
<tr>
<td>
<code>description</code></br>
<em>
string
</em>
</td>
<td>
Description is a human-readable explanation of this template parameter.
</td>
</tr>
<tr>
<td>
<code>default</code></br>
<em>
string
</em>
</td>
<td>
Default, if specified, defines the default value that should be applied if
the build does not specify the value for this parameter.
</td>
</tr>
</tbody>
</table>
<h3 id="SourceSpec">SourceSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#BuildSpec">BuildSpec</a>)
</p>
<p>
SourceSpec defines the input to the Build
</p>
<table>
<thead>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>git</code></br>
<em>
<a href="#GitSourceSpec">
GitSourceSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Git represents source in a Git repository.
</td>
</tr>
<tr>
<td>
<code>gcs</code></br>
<em>
<a href="#GCSSourceSpec">
GCSSourceSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
GCS represents source in Google Cloud Storage.
</td>
</tr>
<tr>
<td>
<code>custom</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#container-v1-core">
Kubernetes core/v1.Container
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Custom indicates that source should be retrieved using a custom
process defined in a container invocation.
</td>
</tr>
<tr>
<td>
<code>subPath</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
SubPath specifies a path within the fetched source which should be
built. This option makes parent directories *inaccessible* to the
build steps. (The specific source type may, in fact, not even fetch
files not in the SubPath.)
</td>
</tr>
<tr>
<td>
<code>name</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
Name is the name of source. This field is used to uniquely identify the
source init containers
Restrictions on the allowed charatcers
Must be a basename (no /)
Must be a valid DNS name (only alphanumeric characters, no _)
https://tools.ietf.org/html/rfc1123#section-2
</td>
</tr>
<tr>
<td>
<code>targetPath</code></br>
<em>
string
</em>
</td>
<td>
TargetPath is the path in workspace directory where the source will be copied.
TargetPath is optional and if its not set source will be copied under workspace.
TargetPath should not be set for custom source.
</td>
</tr>
</tbody>
</table>
<h3 id="Template">Template
</h3>
<p>
Template is an interface for accessing the BuildTemplateSpec
from various forms of template (namespace-/cluster-scoped).
</p>
<h3 id="TemplateInstantiationSpec">TemplateInstantiationSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#BuildSpec">BuildSpec</a>)
</p>
<p>
TemplateInstantiationSpec specifies how a BuildTemplate is instantiated into
a Build.
</p>
<table>
<thead>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>name</code></br>
<em>
string
</em>
</td>
<td>
Name references the BuildTemplate resource to use.
The template is assumed to exist in the Build's namespace.
</td>
</tr>
<tr>
<td>
<code>kind</code></br>
<em>
<a href="#TemplateKind">
TemplateKind
</a>
</em>
</td>
<td>
<em>(Optional)</em>
The Kind of the template to be used, possible values are BuildTemplate
or ClusterBuildTemplate. If nothing is specified, the default if is BuildTemplate
</td>
</tr>
<tr>
<td>
<code>arguments</code></br>
<em>
<a href="#ArgumentSpec">
[]ArgumentSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Arguments, if specified, lists values that should be applied to the
parameters specified by the template.
</td>
</tr>
<tr>
<td>
<code>env</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#envvar-v1-core">
[]Kubernetes core/v1.EnvVar
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Env, if specified will provide variables to all build template steps.
This will override any of the template's steps environment variables.
</td>
</tr>
</tbody>
</table>
<h3 id="TemplateKind">TemplateKind
(<code>string</code> alias)</p></h3>
<p>
(<em>Appears on:</em>
<a href="#TemplateInstantiationSpec">TemplateInstantiationSpec</a>)
</p>
<p>
TemplateKind defines the type of BuildTemplate used by the build.
</p>
<hr/>

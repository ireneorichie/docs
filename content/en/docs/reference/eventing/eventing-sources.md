<h1>API Reference Documentation</h1>
<h2>
sources.eventing.knative.dev
</h2>
<p>
Package v1alpha1 contains API Schema definitions for the sources v1alpha1 API group
</p>
Resource Types:
<ul><li>
<a href="#AwsSqsSource">AwsSqsSource</a>
</li><li>
<a href="#ContainerSource">ContainerSource</a>
</li><li>
<a href="#CronJobSource">CronJobSource</a>
</li><li>
<a href="#GcpPubSubSource">GcpPubSubSource</a>
</li><li>
<a href="#GitHubSource">GitHubSource</a>
</li><li>
<a href="#KubernetesEventSource">KubernetesEventSource</a>
</li></ul>
<h3 id="AwsSqsSource">AwsSqsSource
</h3>
<p>
AwsSqsSource is the Schema for the AWS SQS API
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
sources.eventing.knative.dev/v1alpha1
</code>
</td>
</tr>
<tr>
<td>
<code>kind</code></br>
string
</td>
<td><code>AwsSqsSource</code></td>
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
<a href="#AwsSqsSourceSpec">
AwsSqsSourceSpec
</a>
</em>
</td>
<td>
<br/>
<br/>
<table>
<tr>
<td>
<code>queueUrl</code></br>
<em>
string
</em>
</td>
<td>
QueueURL of the SQS queue that we will poll from.
</td>
</tr>
<tr>
<td>
<code>awsCredsSecret</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#secretkeyselector-v1-core">
Kubernetes core/v1.SecretKeySelector
</a>
</em>
</td>
<td>
AwsCredsSecret is the credential to use to poll the AWS SQS
</td>
</tr>
<tr>
<td>
<code>sink</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectreference-v1-core">
Kubernetes core/v1.ObjectReference
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Sink is a reference to an object that will resolve to a domain name to
use as the sink.  This is where events will be received.
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
ServiceAccoutName is the name of the ServiceAccount that will be used to
run the Receive Adapter Deployment.
</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>
<code>status</code></br>
<em>
<a href="#AwsSqsSourceStatus">
AwsSqsSourceStatus
</a>
</em>
</td>
<td>
</td>
</tr>
</tbody>
</table>
<h3 id="ContainerSource">ContainerSource
</h3>
<p>
ContainerSource is the Schema for the containersources API
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
sources.eventing.knative.dev/v1alpha1
</code>
</td>
</tr>
<tr>
<td>
<code>kind</code></br>
string
</td>
<td><code>ContainerSource</code></td>
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
<a href="#ContainerSourceSpec">
ContainerSourceSpec
</a>
</em>
</td>
<td>
<br/>
<br/>
<table>
<tr>
<td>
<code>image</code></br>
<em>
string
</em>
</td>
<td>
Image is the image to run inside of the container.
</td>
</tr>
<tr>
<td>
<code>args</code></br>
<em>
[]string
</em>
</td>
<td>
Args are passed to the ContainerSpec as they are.
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
Env is the list of environment variables to set in the container.
Cannot be updated.
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
ServiceAccountName is the name of the ServiceAccount to use to run this
source.
</td>
</tr>
<tr>
<td>
<code>sink</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectreference-v1-core">
Kubernetes core/v1.ObjectReference
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Sink is a reference to an object that will resolve to a domain name to use as the sink.
</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>
<code>status</code></br>
<em>
<a href="#ContainerSourceStatus">
ContainerSourceStatus
</a>
</em>
</td>
<td>
</td>
</tr>
</tbody>
</table>
<h3 id="CronJobSource">CronJobSource
</h3>
<p>
CronJobSource is the Schema for the cronjobsources API.
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
sources.eventing.knative.dev/v1alpha1
</code>
</td>
</tr>
<tr>
<td>
<code>kind</code></br>
string
</td>
<td><code>CronJobSource</code></td>
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
<a href="#CronJobSourceSpec">
CronJobSourceSpec
</a>
</em>
</td>
<td>
<br/>
<br/>
<table>
<tr>
<td>
<code>schedule</code></br>
<em>
string
</em>
</td>
<td>
Schedule is the cronjob schedule.
</td>
</tr>
<tr>
<td>
<code>data</code></br>
<em>
string
</em>
</td>
<td>
Data is the data posted to the target function.
</td>
</tr>
<tr>
<td>
<code>sink</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectreference-v1-core">
Kubernetes core/v1.ObjectReference
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Sink is a reference to an object that will resolve to a domain name to use as the sink.
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
ServiceAccoutName is the name of the ServiceAccount that will be used to run the Receive
Adapter Deployment.
</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>
<code>status</code></br>
<em>
<a href="#CronJobSourceStatus">
CronJobSourceStatus
</a>
</em>
</td>
<td>
</td>
</tr>
</tbody>
</table>
<h3 id="GcpPubSubSource">GcpPubSubSource
</h3>
<p>
GcpPubSubSource is the Schema for the gcppubsubsources API.
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
sources.eventing.knative.dev/v1alpha1
</code>
</td>
</tr>
<tr>
<td>
<code>kind</code></br>
string
</td>
<td><code>GcpPubSubSource</code></td>
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
<a href="#GcpPubSubSourceSpec">
GcpPubSubSourceSpec
</a>
</em>
</td>
<td>
<br/>
<br/>
<table>
<tr>
<td>
<code>gcpCredsSecret</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#secretkeyselector-v1-core">
Kubernetes core/v1.SecretKeySelector
</a>
</em>
</td>
<td>
GcpCredsSecret is the credential to use to poll the GCP PubSub Subscription. It is not used
to create or delete the Subscription, only to poll it. The value of the secret entry must be
a service account key in the JSON format (see
https://cloud.google.com/iam/docs/creating-managing-service-account-keys).
</td>
</tr>
<tr>
<td>
<code>googleCloudProject</code></br>
<em>
string
</em>
</td>
<td>
GoogleCloudProject is the ID of the Google Cloud Project that the PubSub Topic exists in.
</td>
</tr>
<tr>
<td>
<code>topic</code></br>
<em>
string
</em>
</td>
<td>
Topic is the ID of the GCP PubSub Topic to Subscribe to. It must be in the form of the
unique identifier within the project, not the entire name. E.g. it must be 'laconia', not
'projects/my-gcp-project/topics/laconia'.
</td>
</tr>
<tr>
<td>
<code>sink</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectreference-v1-core">
Kubernetes core/v1.ObjectReference
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Sink is a reference to an object that will resolve to a domain name to use as the sink.
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
ServiceAccoutName is the name of the ServiceAccount that will be used to run the Receive
Adapter Deployment.
</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>
<code>status</code></br>
<em>
<a href="#GcpPubSubSourceStatus">
GcpPubSubSourceStatus
</a>
</em>
</td>
<td>
</td>
</tr>
</tbody>
</table>
<h3 id="GitHubSource">GitHubSource
</h3>
<p>
GitHubSource is the Schema for the githubsources API
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
sources.eventing.knative.dev/v1alpha1
</code>
</td>
</tr>
<tr>
<td>
<code>kind</code></br>
string
</td>
<td><code>GitHubSource</code></td>
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
<a href="#GitHubSourceSpec">
GitHubSourceSpec
</a>
</em>
</td>
<td>
<br/>
<br/>
<table>
<tr>
<td>
<code>serviceAccountName</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
ServiceAccountName holds the name of the Kubernetes service account
as which the underlying K8s resources should be run. If unspecified
this will default to the "default" service account for the namespace
in which the GitHubSource exists.
</td>
</tr>
<tr>
<td>
<code>ownerAndRepository</code></br>
<em>
string
</em>
</td>
<td>
OwnerAndRepository is the GitHub owner/org and repository to
receive events from. The repository may be left off to receive
events from an entire organization.
Examples:
myuser/project
myorganization
</td>
</tr>
<tr>
<td>
<code>eventTypes</code></br>
<em>
[]string
</em>
</td>
<td>
EventType is the type of event to receive from GitHub. These
correspond to the "Webhook event name" values listed at
https://developer.github.com/v3/activity/events/types/ - ie
"pull_request"
</td>
</tr>
<tr>
<td>
<code>accessToken</code></br>
<em>
<a href="#SecretValueFromSource">
SecretValueFromSource
</a>
</em>
</td>
<td>
AccessToken is the Kubernetes secret containing the GitHub
access token
</td>
</tr>
<tr>
<td>
<code>secretToken</code></br>
<em>
<a href="#SecretValueFromSource">
SecretValueFromSource
</a>
</em>
</td>
<td>
SecretToken is the Kubernetes secret containing the GitHub
secret token
</td>
</tr>
<tr>
<td>
<code>sink</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectreference-v1-core">
Kubernetes core/v1.ObjectReference
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Sink is a reference to an object that will resolve to a domain
name to use as the sink.
</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>
<code>status</code></br>
<em>
<a href="#GitHubSourceStatus">
GitHubSourceStatus
</a>
</em>
</td>
<td>
</td>
</tr>
</tbody>
</table>
<h3 id="KubernetesEventSource">KubernetesEventSource
</h3>
<p>
KubernetesEventSource is the Schema for the kuberneteseventsources API
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
sources.eventing.knative.dev/v1alpha1
</code>
</td>
</tr>
<tr>
<td>
<code>kind</code></br>
string
</td>
<td><code>KubernetesEventSource</code></td>
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
<a href="#KubernetesEventSourceSpec">
KubernetesEventSourceSpec
</a>
</em>
</td>
<td>
<br/>
<br/>
<table>
<tr>
<td>
<code>namespace</code></br>
<em>
string
</em>
</td>
<td>
Namespace that we watch kubernetes events in.
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
ServiceAccountName is the name of the ServiceAccount to use to run this
source.
</td>
</tr>
<tr>
<td>
<code>sink</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectreference-v1-core">
Kubernetes core/v1.ObjectReference
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Sink is a reference to an object that will resolve to a domain name to use
as the sink.
</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>
<code>status</code></br>
<em>
<a href="#KubernetesEventSourceStatus">
KubernetesEventSourceStatus
</a>
</em>
</td>
<td>
</td>
</tr>
</tbody>
</table>
<h3 id="AwsSqsSourceSpec">AwsSqsSourceSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#AwsSqsSource">AwsSqsSource</a>)
</p>
<p>
AwsSqsSourceSpec defines the desired state of the source.
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
<code>queueUrl</code></br>
<em>
string
</em>
</td>
<td>
QueueURL of the SQS queue that we will poll from.
</td>
</tr>
<tr>
<td>
<code>awsCredsSecret</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#secretkeyselector-v1-core">
Kubernetes core/v1.SecretKeySelector
</a>
</em>
</td>
<td>
AwsCredsSecret is the credential to use to poll the AWS SQS
</td>
</tr>
<tr>
<td>
<code>sink</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectreference-v1-core">
Kubernetes core/v1.ObjectReference
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Sink is a reference to an object that will resolve to a domain name to
use as the sink.  This is where events will be received.
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
ServiceAccoutName is the name of the ServiceAccount that will be used to
run the Receive Adapter Deployment.
</td>
</tr>
</tbody>
</table>
<h3 id="AwsSqsSourceStatus">AwsSqsSourceStatus
</h3>
<p>
(<em>Appears on:</em>
<a href="#AwsSqsSource">AwsSqsSource</a>)
</p>
<p>
AwsSqsSourceStatus defines the observed state of the source.
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
<code>conditions</code></br>
<em>
<a href="https://godoc.org/github.com/knative/pkg/apis/duck/v1alpha1#Conditions">
github.com/knative/pkg/apis/duck/v1alpha1.Conditions
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Conditions holds the state of a source at a point in time.
</td>
</tr>
<tr>
<td>
<code>sinkUri</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
SinkURI is the current active sink URI that has been configured for the source.
</td>
</tr>
</tbody>
</table>
<h3 id="ContainerSourceSpec">ContainerSourceSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#ContainerSource">ContainerSource</a>)
</p>
<p>
ContainerSourceSpec defines the desired state of ContainerSource
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
<code>image</code></br>
<em>
string
</em>
</td>
<td>
Image is the image to run inside of the container.
</td>
</tr>
<tr>
<td>
<code>args</code></br>
<em>
[]string
</em>
</td>
<td>
Args are passed to the ContainerSpec as they are.
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
Env is the list of environment variables to set in the container.
Cannot be updated.
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
ServiceAccountName is the name of the ServiceAccount to use to run this
source.
</td>
</tr>
<tr>
<td>
<code>sink</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectreference-v1-core">
Kubernetes core/v1.ObjectReference
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Sink is a reference to an object that will resolve to a domain name to use as the sink.
</td>
</tr>
</tbody>
</table>
<h3 id="ContainerSourceStatus">ContainerSourceStatus
</h3>
<p>
(<em>Appears on:</em>
<a href="#ContainerSource">ContainerSource</a>)
</p>
<p>
ContainerSourceStatus defines the observed state of ContainerSource
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
<code>conditions</code></br>
<em>
<a href="https://godoc.org/github.com/knative/pkg/apis/duck/v1alpha1#Conditions">
github.com/knative/pkg/apis/duck/v1alpha1.Conditions
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Conditions holds the state of a source at a point in time.
</td>
</tr>
<tr>
<td>
<code>sinkUri</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
SinkURI is the current active sink URI that has been configured for the ContainerSource.
</td>
</tr>
</tbody>
</table>
<h3 id="CronJobSourceSpec">CronJobSourceSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#CronJobSource">CronJobSource</a>)
</p>
<p>
CronJobSourceSpec defines the desired state of the CronJobSource.
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
<code>schedule</code></br>
<em>
string
</em>
</td>
<td>
Schedule is the cronjob schedule.
</td>
</tr>
<tr>
<td>
<code>data</code></br>
<em>
string
</em>
</td>
<td>
Data is the data posted to the target function.
</td>
</tr>
<tr>
<td>
<code>sink</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectreference-v1-core">
Kubernetes core/v1.ObjectReference
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Sink is a reference to an object that will resolve to a domain name to use as the sink.
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
ServiceAccoutName is the name of the ServiceAccount that will be used to run the Receive
Adapter Deployment.
</td>
</tr>
</tbody>
</table>
<h3 id="CronJobSourceStatus">CronJobSourceStatus
</h3>
<p>
(<em>Appears on:</em>
<a href="#CronJobSource">CronJobSource</a>)
</p>
<p>
CronJobSourceStatus defines the observed state of CronJobSource.
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
<code>conditions</code></br>
<em>
<a href="https://godoc.org/github.com/knative/pkg/apis/duck/v1alpha1#Conditions">
github.com/knative/pkg/apis/duck/v1alpha1.Conditions
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Conditions holds the state of a source at a point in time.
</td>
</tr>
<tr>
<td>
<code>sinkUri</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
SinkURI is the current active sink URI that has been configured for the CronJobSource.
</td>
</tr>
</tbody>
</table>
<h3 id="GcpPubSubSourceSpec">GcpPubSubSourceSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#GcpPubSubSource">GcpPubSubSource</a>)
</p>
<p>
GcpPubSubSourceSpec defines the desired state of the GcpPubSubSource.
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
<code>gcpCredsSecret</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#secretkeyselector-v1-core">
Kubernetes core/v1.SecretKeySelector
</a>
</em>
</td>
<td>
GcpCredsSecret is the credential to use to poll the GCP PubSub Subscription. It is not used
to create or delete the Subscription, only to poll it. The value of the secret entry must be
a service account key in the JSON format (see
https://cloud.google.com/iam/docs/creating-managing-service-account-keys).
</td>
</tr>
<tr>
<td>
<code>googleCloudProject</code></br>
<em>
string
</em>
</td>
<td>
GoogleCloudProject is the ID of the Google Cloud Project that the PubSub Topic exists in.
</td>
</tr>
<tr>
<td>
<code>topic</code></br>
<em>
string
</em>
</td>
<td>
Topic is the ID of the GCP PubSub Topic to Subscribe to. It must be in the form of the
unique identifier within the project, not the entire name. E.g. it must be 'laconia', not
'projects/my-gcp-project/topics/laconia'.
</td>
</tr>
<tr>
<td>
<code>sink</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectreference-v1-core">
Kubernetes core/v1.ObjectReference
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Sink is a reference to an object that will resolve to a domain name to use as the sink.
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
ServiceAccoutName is the name of the ServiceAccount that will be used to run the Receive
Adapter Deployment.
</td>
</tr>
</tbody>
</table>
<h3 id="GcpPubSubSourceStatus">GcpPubSubSourceStatus
</h3>
<p>
(<em>Appears on:</em>
<a href="#GcpPubSubSource">GcpPubSubSource</a>)
</p>
<p>
GcpPubSubSourceStatus defines the observed state of GcpPubSubSource.
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
<code>conditions</code></br>
<em>
<a href="https://godoc.org/github.com/knative/pkg/apis/duck/v1alpha1#Conditions">
github.com/knative/pkg/apis/duck/v1alpha1.Conditions
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Conditions holds the state of a source at a point in time.
</td>
</tr>
<tr>
<td>
<code>sinkUri</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
SinkURI is the current active sink URI that has been configured for the GcpPubSubSource.
</td>
</tr>
</tbody>
</table>
<h3 id="GitHubSourceSpec">GitHubSourceSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#GitHubSource">GitHubSource</a>)
</p>
<p>
GitHubSourceSpec defines the desired state of GitHubSource
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
<code>serviceAccountName</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
ServiceAccountName holds the name of the Kubernetes service account
as which the underlying K8s resources should be run. If unspecified
this will default to the "default" service account for the namespace
in which the GitHubSource exists.
</td>
</tr>
<tr>
<td>
<code>ownerAndRepository</code></br>
<em>
string
</em>
</td>
<td>
OwnerAndRepository is the GitHub owner/org and repository to
receive events from. The repository may be left off to receive
events from an entire organization.
Examples:
myuser/project
myorganization
</td>
</tr>
<tr>
<td>
<code>eventTypes</code></br>
<em>
[]string
</em>
</td>
<td>
EventType is the type of event to receive from GitHub. These
correspond to the "Webhook event name" values listed at
https://developer.github.com/v3/activity/events/types/ - ie
"pull_request"
</td>
</tr>
<tr>
<td>
<code>accessToken</code></br>
<em>
<a href="#SecretValueFromSource">
SecretValueFromSource
</a>
</em>
</td>
<td>
AccessToken is the Kubernetes secret containing the GitHub
access token
</td>
</tr>
<tr>
<td>
<code>secretToken</code></br>
<em>
<a href="#SecretValueFromSource">
SecretValueFromSource
</a>
</em>
</td>
<td>
SecretToken is the Kubernetes secret containing the GitHub
secret token
</td>
</tr>
<tr>
<td>
<code>sink</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectreference-v1-core">
Kubernetes core/v1.ObjectReference
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Sink is a reference to an object that will resolve to a domain
name to use as the sink.
</td>
</tr>
</tbody>
</table>
<h3 id="GitHubSourceStatus">GitHubSourceStatus
</h3>
<p>
(<em>Appears on:</em>
<a href="#GitHubSource">GitHubSource</a>)
</p>
<p>
GitHubSourceStatus defines the observed state of GitHubSource
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
<code>conditions</code></br>
<em>
<a href="https://godoc.org/github.com/knative/pkg/apis/duck/v1alpha1#Conditions">
github.com/knative/pkg/apis/duck/v1alpha1.Conditions
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Conditions holds the state of a source at a point in time.
</td>
</tr>
<tr>
<td>
<code>webhookIDKey</code></br>
<em>
string
</em>
</td>
<td>
WebhookIDKey is the ID of the webhook registered with GitHub
</td>
</tr>
<tr>
<td>
<code>sinkUri</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
SinkURI is the current active sink URI that has been configured
for the GitHubSource.
</td>
</tr>
</tbody>
</table>
<h3 id="KubernetesEventSourceSpec">KubernetesEventSourceSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#KubernetesEventSource">KubernetesEventSource</a>)
</p>
<p>
KubernetesEventSourceSpec defines the desired state of the source.
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
Namespace that we watch kubernetes events in.
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
ServiceAccountName is the name of the ServiceAccount to use to run this
source.
</td>
</tr>
<tr>
<td>
<code>sink</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectreference-v1-core">
Kubernetes core/v1.ObjectReference
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Sink is a reference to an object that will resolve to a domain name to use
as the sink.
</td>
</tr>
</tbody>
</table>
<h3 id="KubernetesEventSourceStatus">KubernetesEventSourceStatus
</h3>
<p>
(<em>Appears on:</em>
<a href="#KubernetesEventSource">KubernetesEventSource</a>)
</p>
<p>
KubernetesEventSourceStatus defines the observed state of the source.
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
<code>conditions</code></br>
<em>
<a href="https://godoc.org/github.com/knative/pkg/apis/duck/v1alpha1#Conditions">
github.com/knative/pkg/apis/duck/v1alpha1.Conditions
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Conditions holds the state of a source at a point in time.
</td>
</tr>
<tr>
<td>
<code>sinkUri</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
SinkURI is the current active sink URI that has been configured for the source.
</td>
</tr>
</tbody>
</table>
<h3 id="SecretValueFromSource">SecretValueFromSource
</h3>
<p>
(<em>Appears on:</em>
<a href="#GitHubSourceSpec">GitHubSourceSpec</a>)
</p>
<p>
SecretValueFromSource represents the source of a secret value
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
<code>secretKeyRef</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#secretkeyselector-v1-core">
Kubernetes core/v1.SecretKeySelector
</a>
</em>
</td>
<td>
The Secret key to select from.
</td>
</tr>
</tbody>
</table>
<hr/>

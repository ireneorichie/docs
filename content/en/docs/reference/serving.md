<h1>API Reference Documentation</h1>
<h2>
networking.internal.knative.dev
</h2>
<p>
</p>
Resource Types:
<ul><li>
<a href="#ClusterIngress">ClusterIngress</a>
</li></ul>
<h3 id="ClusterIngress">ClusterIngress
</h3>
<p>
ClusterIngress is a collection of rules that allow inbound connections to reach the
endpoints defined by a backend. An ClusterIngress can be configured to give services
externally-reachable urls, load balance traffic offer name based virtual hosting etc.<br/><br/>This is heavily based on K8s Ingress https://godoc.org/k8s.io/api/extensions/v1beta1#Ingress
which some highlighted modifications.
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
networking.internal.knative.dev/v1alpha1
</code>
</td>
</tr>
<tr>
<td>
<code>kind</code></br>
string
</td>
<td><code>ClusterIngress</code></td>
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
<em>(Optional)</em>
Standard object's metadata.
More info: https://git.k8s.io/community/contributors/devel/api-conventions.md#metadata
Refer to the Kubernetes API documentation for the fields of the
<code>metadata</code> field.
</td>
</tr>
<tr>
<td>
<code>spec</code></br>
<em>
<a href="#IngressSpec">
IngressSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Spec is the desired state of the ClusterIngress.
More info: https://git.k8s.io/community/contributors/devel/api-conventions.md#spec-and-status
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
<code>tls</code></br>
<em>
<a href="#ClusterIngressTLS">
[]ClusterIngressTLS
</a>
</em>
</td>
<td>
<em>(Optional)</em>
TLS configuration. Currently the ClusterIngress only supports a single TLS
port, 443. If multiple members of this list specify different hosts, they
will be multiplexed on the same port according to the hostname specified
through the SNI TLS extension, if the ingress controller fulfilling the
ingress supports SNI.
</td>
</tr>
<tr>
<td>
<code>rules</code></br>
<em>
<a href="#ClusterIngressRule">
[]ClusterIngressRule
</a>
</em>
</td>
<td>
<em>(Optional)</em>
A list of host rules used to configure the ClusterIngress.
</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>
<code>status</code></br>
<em>
<a href="#IngressStatus">
IngressStatus
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Status is the current state of the ClusterIngress.
More info: https://git.k8s.io/community/contributors/devel/api-conventions.md#spec-and-status
</td>
</tr>
</tbody>
</table>
<h3 id="ClusterIngressBackend">ClusterIngressBackend
</h3>
<p>
(<em>Appears on:</em>
<a href="#ClusterIngressBackendSplit">ClusterIngressBackendSplit</a>)
</p>
<p>
ClusterIngressBackend describes all endpoints for a given service and port.
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
<code>serviceNamespace</code></br>
<em>
string
</em>
</td>
<td>
Specifies the namespace of the referenced service.<br/><br/>NOTE: This differs from K8s Ingress to allow routing to different namespaces.
</td>
</tr>
<tr>
<td>
<code>serviceName</code></br>
<em>
string
</em>
</td>
<td>
Specifies the name of the referenced service.
</td>
</tr>
<tr>
<td>
<code>servicePort</code></br>
<em>
k8s.io/apimachinery/pkg/util/intstr.IntOrString
</em>
</td>
<td>
Specifies the port of the referenced service.
</td>
</tr>
</tbody>
</table>
<h3 id="ClusterIngressBackendSplit">ClusterIngressBackendSplit
</h3>
<p>
(<em>Appears on:</em>
<a href="#HTTPClusterIngressPath">HTTPClusterIngressPath</a>)
</p>
<p>
ClusterIngressBackend describes all endpoints for a given service and port.
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
<code>ClusterIngressBackend</code></br>
<em>
<a href="#ClusterIngressBackend">
ClusterIngressBackend
</a>
</em>
</td>
<td>
<p>
(Members of <code>ClusterIngressBackend</code> are embedded into this type.)
</p>
Specifies the backend receiving the traffic split.
</td>
</tr>
<tr>
<td>
<code>percent</code></br>
<em>
int
</em>
</td>
<td>
Specifies the split percentage, a number between 0 and 100.  If
only one split is specified, we default to 100.<br/><br/>NOTE: This differs from K8s Ingress to allow percentage split.
</td>
</tr>
</tbody>
</table>
<h3 id="ClusterIngressRule">ClusterIngressRule
</h3>
<p>
(<em>Appears on:</em>
<a href="#IngressSpec">IngressSpec</a>)
</p>
<p>
ClusterIngressRule represents the rules mapping the paths under a specified host to
the related backend services. Incoming requests are first evaluated for a host
match, then routed to the backend associated with the matching ClusterIngressRuleValue.
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
<code>hosts</code></br>
<em>
[]string
</em>
</td>
<td>
<em>(Optional)</em>
Host is the fully qualified domain name of a network host, as defined
by RFC 3986. Note the following deviations from the "host" part of the
URI as defined in the RFC:
1. IPs are not allowed. Currently a rule value can only apply to the
IP in the Spec of the parent ClusterIngress.
2. The `:` delimiter is not respected because ports are not allowed.
Currently the port of an ClusterIngress is implicitly :80 for http and
:443 for https.
Both these may change in the future.
If the host is unspecified, the ClusterIngress routes all traffic based on the
specified ClusterIngressRuleValue.
If multiple matching Hosts were provided, the first rule will take precedent.
</td>
</tr>
<tr>
<td>
<code>http</code></br>
<em>
<a href="#HTTPClusterIngressRuleValue">
HTTPClusterIngressRuleValue
</a>
</em>
</td>
<td>
HTTP represents a rule to apply against incoming requests. If the
rule is satisfied, the request is routed to the specified backend.
</td>
</tr>
</tbody>
</table>
<h3 id="ClusterIngressTLS">ClusterIngressTLS
</h3>
<p>
(<em>Appears on:</em>
<a href="#IngressSpec">IngressSpec</a>)
</p>
<p>
ClusterIngressTLS describes the transport layer security associated with an ClusterIngress.
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
<code>hosts</code></br>
<em>
[]string
</em>
</td>
<td>
<em>(Optional)</em>
Hosts are a list of hosts included in the TLS certificate. The values in
this list must match the name/s used in the tlsSecret. Defaults to the
wildcard host setting for the loadbalancer controller fulfilling this
ClusterIngress, if left unspecified.
</td>
</tr>
<tr>
<td>
<code>secretName</code></br>
<em>
string
</em>
</td>
<td>
SecretName is the name of the secret used to terminate SSL traffic.
</td>
</tr>
<tr>
<td>
<code>secretNamespace</code></br>
<em>
string
</em>
</td>
<td>
SecretNamespace is the namespace of the secret used to terminate SSL traffic.
</td>
</tr>
<tr>
<td>
<code>serverCertificate</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
ServerCertificate identifies the certificate filename in the secret.
Defaults to `tls.cert`.
</td>
</tr>
<tr>
<td>
<code>privateKey</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
PrivateKey identifies the private key filename in the secret.
Defaults to `tls.key`.
</td>
</tr>
</tbody>
</table>
<h3 id="HTTPClusterIngressPath">HTTPClusterIngressPath
</h3>
<p>
(<em>Appears on:</em>
<a href="#HTTPClusterIngressRuleValue">HTTPClusterIngressRuleValue</a>)
</p>
<p>
HTTPClusterIngressPath associates a path regex with a backend. Incoming urls matching
the path are forwarded to the backend.
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
<code>path</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
Path is an extended POSIX regex as defined by IEEE Std 1003.1,
(i.e this follows the egrep/unix syntax, not the perl syntax)
matched against the path of an incoming request. Currently it can
contain characters disallowed from the conventional "path"
part of a URL as defined by RFC 3986. Paths must begin with
a '/'. If unspecified, the path defaults to a catch all sending
traffic to the backend.
</td>
</tr>
<tr>
<td>
<code>splits</code></br>
<em>
<a href="#ClusterIngressBackendSplit">
[]ClusterIngressBackendSplit
</a>
</em>
</td>
<td>
Splits defines the referenced service endpoints to which the traffic
will be forwarded to.
</td>
</tr>
<tr>
<td>
<code>appendHeaders</code></br>
<em>
map[string]string
</em>
</td>
<td>
<em>(Optional)</em>
AppendHeaders allow specifying additional HTTP headers to add
before forwarding a request to the destination service.<br/><br/>NOTE: This differs from K8s Ingress which doesn't allow header appending.
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
Timeout for HTTP requests.<br/><br/>NOTE: This differs from K8s Ingress which doesn't allow setting timeouts.
</td>
</tr>
<tr>
<td>
<code>retries</code></br>
<em>
<a href="#HTTPRetry">
HTTPRetry
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Retry policy for HTTP requests.<br/><br/>NOTE: This differs from K8s Ingress which doesn't allow retry settings.
</td>
</tr>
</tbody>
</table>
<h3 id="HTTPClusterIngressRuleValue">HTTPClusterIngressRuleValue
</h3>
<p>
(<em>Appears on:</em>
<a href="#ClusterIngressRule">ClusterIngressRule</a>)
</p>
<p>
HTTPClusterIngressRuleValue is a list of http selectors pointing to backends.
In the example: http://<host>/<path>?<searchpart> -> backend where
where parts of the url correspond to RFC 3986, this resource will be used
to match against everything after the last '/' and before the first '?'
or '#'.
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
<code>paths</code></br>
<em>
<a href="#HTTPClusterIngressPath">
[]HTTPClusterIngressPath
</a>
</em>
</td>
<td>
A collection of paths that map requests to backends.<br/><br/>If they are multiple matching paths, the first match takes precendent.
</td>
</tr>
</tbody>
</table>
<h3 id="HTTPRetry">HTTPRetry
</h3>
<p>
(<em>Appears on:</em>
<a href="#HTTPClusterIngressPath">HTTPClusterIngressPath</a>)
</p>
<p>
HTTPRetry describes the retry policy to use when a HTTP request fails.
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
<code>attempts</code></br>
<em>
int
</em>
</td>
<td>
Number of retries for a given request.
</td>
</tr>
<tr>
<td>
<code>perTryTimeout</code></br>
<em>
<a href="https://godoc.org/k8s.io/apimachinery/pkg/apis/meta/v1#Duration">
Kubernetes meta/v1.Duration
</a>
</em>
</td>
<td>
Timeout per retry attempt for a given request. format: 1h/1m/1s/1ms. MUST BE >=1ms.
</td>
</tr>
</tbody>
</table>
<h3 id="IngressSpec">IngressSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#ClusterIngress">ClusterIngress</a>)
</p>
<p>
IngressSpec describes the ClusterIngress the user wishes to exist.<br/><br/>In general this follow the same shape as K8s Ingress.  Some notable differences:
- Backends now can have namespace:
- Traffic can be split across multiple backends.
- Timeout & Retry can be configured.
- Headers can be appended.
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
<code>tls</code></br>
<em>
<a href="#ClusterIngressTLS">
[]ClusterIngressTLS
</a>
</em>
</td>
<td>
<em>(Optional)</em>
TLS configuration. Currently the ClusterIngress only supports a single TLS
port, 443. If multiple members of this list specify different hosts, they
will be multiplexed on the same port according to the hostname specified
through the SNI TLS extension, if the ingress controller fulfilling the
ingress supports SNI.
</td>
</tr>
<tr>
<td>
<code>rules</code></br>
<em>
<a href="#ClusterIngressRule">
[]ClusterIngressRule
</a>
</em>
</td>
<td>
<em>(Optional)</em>
A list of host rules used to configure the ClusterIngress.
</td>
</tr>
</tbody>
</table>
<h3 id="IngressStatus">IngressStatus
</h3>
<p>
(<em>Appears on:</em>
<a href="#ClusterIngress">ClusterIngress</a>)
</p>
<p>
IngressStatus describe the current state of the ClusterIngress.
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
</td>
</tr>
<tr>
<td>
<code>loadBalancer</code></br>
<em>
<a href="#LoadBalancerStatus">
LoadBalancerStatus
</a>
</em>
</td>
<td>
<em>(Optional)</em>
LoadBalancer contains the current status of the load-balancer.
</td>
</tr>
</tbody>
</table>
<h3 id="LoadBalancerIngressStatus">LoadBalancerIngressStatus
</h3>
<p>
(<em>Appears on:</em>
<a href="#LoadBalancerStatus">LoadBalancerStatus</a>)
</p>
<p>
LoadBalancerIngress represents the status of a load-balancer ingress point:
traffic intended for the service should be sent to an ingress point.
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
<code>ip</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
IP is set for load-balancer ingress points that are IP based
(typically GCE or OpenStack load-balancers)
</td>
</tr>
<tr>
<td>
<code>domain</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
Domain is set for load-balancer ingress points that are DNS based
(typically AWS load-balancers)
</td>
</tr>
<tr>
<td>
<code>domainInternal</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
DomainInternal is set if there is a cluster-local DNS name to access the Ingress.<br/><br/>NOTE: This differs from K8s Ingress, since we also desire to have a cluster-local
DNS name to allow routing in case of not having a mesh.
</td>
</tr>
</tbody>
</table>
<h3 id="LoadBalancerStatus">LoadBalancerStatus
</h3>
<p>
(<em>Appears on:</em>
<a href="#IngressStatus">IngressStatus</a>)
</p>
<p>
LoadBalancerStatus represents the status of a load-balancer.
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
<code>ingress</code></br>
<em>
<a href="#LoadBalancerIngressStatus">
[]LoadBalancerIngressStatus
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Ingress is a list containing ingress points for the load-balancer.
Traffic intended for the service should be sent to these ingress points.
</td>
</tr>
</tbody>
</table>
<hr/>
<h2>
serving.knative.dev
</h2>
<p>
</p>
Resource Types:
<ul><li>
<a href="#Configuration">Configuration</a>
</li><li>
<a href="#Revision">Revision</a>
</li><li>
<a href="#Route">Route</a>
</li><li>
<a href="#Service">Service</a>
</li></ul>
<h3 id="Configuration">Configuration
</h3>
<p>
Configuration represents the "floating HEAD" of a linear history of Revisions,
and optionally how the containers those revisions reference are built.
Users create new Revisions by updating the Configuration's spec.
The "latest created" revision's name is available under status, as is the
"latest ready" revision's name.
See also: https://github.com/knative/serving/blob/master/docs/spec/overview.md#configuration
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
serving.knative.dev/v1alpha1
</code>
</td>
</tr>
<tr>
<td>
<code>kind</code></br>
string
</td>
<td><code>Configuration</code></td>
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
<em>(Optional)</em>
Refer to the Kubernetes API documentation for the fields of the
<code>metadata</code> field.
</td>
</tr>
<tr>
<td>
<code>spec</code></br>
<em>
<a href="#ConfigurationSpec">
ConfigurationSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Spec holds the desired state of the Configuration (from the client).
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
<code>build</code></br>
<em>
<a href="#RawExtension">
RawExtension
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Build optionally holds the specification for the build to
perform to produce the Revision's container image.
</td>
</tr>
<tr>
<td>
<code>revisionTemplate</code></br>
<em>
<a href="#RevisionTemplateSpec">
RevisionTemplateSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
RevisionTemplate holds the latest specification for the Revision to
be stamped out. If a Build specification is provided, then the
RevisionTemplate's BuildName field will be populated with the name of
the Build object created to produce the container for the Revision.
</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>
<code>status</code></br>
<em>
<a href="#ConfigurationStatus">
ConfigurationStatus
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Status communicates the observed state of the Configuration (from the controller).
</td>
</tr>
</tbody>
</table>
<h3 id="Revision">Revision
</h3>
<p>
Revision is an immutable snapshot of code and configuration.  A revision
references a container image, and optionally a build that is responsible for
materializing that container image from source. Revisions are created by
updates to a Configuration.<br/><br/>See also: https://github.com/knative/serving/blob/master/docs/spec/overview.md#revision
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
serving.knative.dev/v1alpha1
</code>
</td>
</tr>
<tr>
<td>
<code>kind</code></br>
string
</td>
<td><code>Revision</code></td>
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
<em>(Optional)</em>
Refer to the Kubernetes API documentation for the fields of the
<code>metadata</code> field.
</td>
</tr>
<tr>
<td>
<code>spec</code></br>
<em>
<a href="#RevisionSpec">
RevisionSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Spec holds the desired state of the Revision (from the client).
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
<code>servingState</code></br>
<em>
<a href="#DeprecatedRevisionServingStateType">
DeprecatedRevisionServingStateType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
DeprecatedServingState holds a value describing the desired state the Kubernetes
resources should be in for this Revision.
Users must not specify this when creating a revision. These values are no longer
updated by the system.
</td>
</tr>
<tr>
<td>
<code>concurrencyModel</code></br>
<em>
<a href="#RevisionRequestConcurrencyModelType">
RevisionRequestConcurrencyModelType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
ConcurrencyModel specifies the desired concurrency model
(Single or Multi) for the
Revision. Defaults to Multi.
Deprecated in favor of ContainerConcurrency.
</td>
</tr>
<tr>
<td>
<code>containerConcurrency</code></br>
<em>
<a href="#RevisionContainerConcurrencyType">
RevisionContainerConcurrencyType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
ContainerConcurrency specifies the maximum allowed
in-flight (concurrent) requests per container of the Revision.
Defaults to `0` which means unlimited concurrency.
This field replaces ConcurrencyModel. A value of `1`
is equivalent to `Single` and `0` is equivalent to `Multi`.
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
ServiceAccountName holds the name of the Kubernetes service account
as which the underlying K8s resources should be run. If unspecified
this will default to the "default" service account for the namespace
in which the Revision exists.
This may be used to provide access to private container images by
following: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/#add-imagepullsecrets-to-a-service-account
TODO(ZhiminXiang): verify the corresponding service account exists.
</td>
</tr>
<tr>
<td>
<code>buildName</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
BuildName optionally holds the name of the Build responsible for
producing the container image for its Revision.
DEPRECATED: Use BuildRef instead.
</td>
</tr>
<tr>
<td>
<code>buildRef</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectreference-v1-core">
Kubernetes core/v1.ObjectReference
</a>
</em>
</td>
<td>
<em>(Optional)</em>
BuildRef holds the reference to the build (if there is one) responsible
for producing the container image for this Revision. Otherwise, nil
</td>
</tr>
<tr>
<td>
<code>container</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#container-v1-core">
Kubernetes core/v1.Container
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Container defines the unit of execution for this Revision.
In the context of a Revision, we disallow a number of the fields of
this Container, including: name, resources, ports, and volumeMounts.
TODO(mattmoor): Link to the runtime contract tracked by:
https://github.com/knative/serving/issues/627
</td>
</tr>
<tr>
<td>
<code>timeoutSeconds</code></br>
<em>
<a href="https://godoc.org/k8s.io/apimachinery/pkg/apis/meta/v1#Duration">
Kubernetes meta/v1.Duration
</a>
</em>
</td>
<td>
<em>(Optional)</em>
TimeoutSeconds holds the max duration the instance is allowed for responding to a request.
</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>
<code>status</code></br>
<em>
<a href="#RevisionStatus">
RevisionStatus
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Status communicates the observed state of the Revision (from the controller).
</td>
</tr>
</tbody>
</table>
<h3 id="Route">Route
</h3>
<p>
Route is responsible for configuring ingress over a collection of Revisions.
Some of the Revisions a Route distributes traffic over may be specified by
referencing the Configuration responsible for creating them; in these cases
the Route is additionally responsible for monitoring the Configuration for
"latest ready" revision changes, and smoothly rolling out latest revisions.
See also: https://github.com/knative/serving/blob/master/docs/spec/overview.md#route
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
serving.knative.dev/v1alpha1
</code>
</td>
</tr>
<tr>
<td>
<code>kind</code></br>
string
</td>
<td><code>Route</code></td>
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
<em>(Optional)</em>
Refer to the Kubernetes API documentation for the fields of the
<code>metadata</code> field.
</td>
</tr>
<tr>
<td>
<code>spec</code></br>
<em>
<a href="#RouteSpec">
RouteSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Spec holds the desired state of the Route (from the client).
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
<code>traffic</code></br>
<em>
<a href="#TrafficTarget">
[]TrafficTarget
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Traffic specifies how to distribute traffic over a collection of Knative Serving Revisions and Configurations.
</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>
<code>status</code></br>
<em>
<a href="#RouteStatus">
RouteStatus
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Status communicates the observed state of the Route (from the controller).
</td>
</tr>
</tbody>
</table>
<h3 id="Service">Service
</h3>
<p>
Service acts as a top-level container that manages a set of Routes and
Configurations which implement a network service. Service exists to provide a
singular abstraction which can be access controlled, reasoned about, and
which encapsulates software lifecycle decisions such as rollout policy and
team resource ownership. Service acts only as an orchestrator of the
underlying Routes and Configurations (much as a kubernetes Deployment
orchestrates ReplicaSets), and its usage is optional but recommended.<br/><br/>The Service's controller will track the statuses of its owned Configuration
and Route, reflecting their statuses and conditions as its own.<br/><br/>See also: https://github.com/knative/serving/blob/master/docs/spec/overview.md#service
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
serving.knative.dev/v1alpha1
</code>
</td>
</tr>
<tr>
<td>
<code>kind</code></br>
string
</td>
<td><code>Service</code></td>
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
<em>(Optional)</em>
Refer to the Kubernetes API documentation for the fields of the
<code>metadata</code> field.
</td>
</tr>
<tr>
<td>
<code>spec</code></br>
<em>
<a href="#ServiceSpec">
ServiceSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
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
<code>runLatest</code></br>
<em>
<a href="#RunLatestType">
RunLatestType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
RunLatest defines a simple Service. It will automatically
configure a route that keeps the latest ready revision
from the supplied configuration running.
</td>
</tr>
<tr>
<td>
<code>pinned</code></br>
<em>
<a href="#PinnedType">
PinnedType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Pins this service to a specific revision name. The revision must
be owned by the configuration provided.
PinnedType is DEPRECATED in favor of ReleaseType
</td>
</tr>
<tr>
<td>
<code>manual</code></br>
<em>
<a href="#ManualType">
ManualType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Manual mode enables users to start managing the underlying Route and Configuration
resources directly.  This advanced usage is intended as a path for users to graduate
from the limited capabilities of Service to the full power of Route.
</td>
</tr>
<tr>
<td>
<code>release</code></br>
<em>
<a href="#ReleaseType">
ReleaseType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Release enables gradual promotion of new revisions by allowing traffic
to be split between two revisions. This type replaces the deprecated Pinned type.
</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>
<code>status</code></br>
<em>
<a href="#ServiceStatus">
ServiceStatus
</a>
</em>
</td>
<td>
<em>(Optional)</em>
</td>
</tr>
</tbody>
</table>
<h3 id="ConfigurationSpec">ConfigurationSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#RunLatestType">RunLatestType</a>,
<a href="#ReleaseType">ReleaseType</a>,
<a href="#PinnedType">PinnedType</a>,
<a href="#Configuration">Configuration</a>)
</p>
<p>
ConfigurationSpec holds the desired state of the Configuration (from the client).
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
<code>build</code></br>
<em>
<a href="#RawExtension">
RawExtension
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Build optionally holds the specification for the build to
perform to produce the Revision's container image.
</td>
</tr>
<tr>
<td>
<code>revisionTemplate</code></br>
<em>
<a href="#RevisionTemplateSpec">
RevisionTemplateSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
RevisionTemplate holds the latest specification for the Revision to
be stamped out. If a Build specification is provided, then the
RevisionTemplate's BuildName field will be populated with the name of
the Build object created to produce the container for the Revision.
</td>
</tr>
</tbody>
</table>
<h3 id="ConfigurationStatus">ConfigurationStatus
</h3>
<p>
(<em>Appears on:</em>
<a href="#Configuration">Configuration</a>)
</p>
<p>
ConfigurationStatus communicates the observed state of the Configuration (from the controller).
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
Conditions communicates information about ongoing/complete
reconciliation processes that bring the "spec" inline with the observed
state of the world.
</td>
</tr>
<tr>
<td>
<code>latestReadyRevisionName</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
LatestReadyRevisionName holds the name of the latest Revision stamped out
from this Configuration that has had its "Ready" condition become "True".
</td>
</tr>
<tr>
<td>
<code>latestCreatedRevisionName</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
LatestCreatedRevisionName is the last revision that was created from this
Configuration. It might not be ready yet, for that use LatestReadyRevisionName.
</td>
</tr>
<tr>
<td>
<code>observedGeneration</code></br>
<em>
int64
</em>
</td>
<td>
<em>(Optional)</em>
ObservedGeneration is the 'Generation' of the Configuration that
was last processed by the controller. The observed generation is updated
even if the controller failed to process the spec and create the Revision.
</td>
</tr>
</tbody>
</table>
<h3 id="DeprecatedRevisionServingStateType">DeprecatedRevisionServingStateType
(<code>string</code> alias)</p></h3>
<p>
(<em>Appears on:</em>
<a href="#RevisionSpec">RevisionSpec</a>)
</p>
<p>
DeprecatedRevisionServingStateType is an enumeration of the levels of serving readiness of the Revision.
See also: https://github.com/knative/serving/blob/master/docs/spec/errors.md#error-conditions-and-reporting
</p>
<h3 id="ManualType">ManualType
</h3>
<p>
(<em>Appears on:</em>
<a href="#ServiceSpec">ServiceSpec</a>)
</p>
<p>
ManualType contains the options for configuring a manual service. See ServiceSpec for
more details.
</p>
<h3 id="PinnedType">PinnedType
</h3>
<p>
(<em>Appears on:</em>
<a href="#ServiceSpec">ServiceSpec</a>)
</p>
<p>
PinnedType is DEPRECATED. ReleaseType should be used instead. To get the behavior of PinnedType set
ReleaseType.Revisions to []string{PinnedType.RevisionName} and ReleaseType.RolloutPercent to 0.
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
<code>revisionName</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
The revision name to pin this service to until changed
to a different service type.
</td>
</tr>
<tr>
<td>
<code>configuration</code></br>
<em>
<a href="#ConfigurationSpec">
ConfigurationSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
The configuration for this service.
</td>
</tr>
</tbody>
</table>
<h3 id="RawExtension">RawExtension
</h3>
<p>
(<em>Appears on:</em>
<a href="#ConfigurationSpec">ConfigurationSpec</a>)
</p>
<p>
RawExtension is modeled after runtime.RawExtension, and should be
replaced with it (or an alias) once we can stop supporting embedded
BuildSpecs.
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
<code>Raw</code></br>
<em>
[]byte
</em>
</td>
<td>
Field order is the precedence for JSON marshaling if multiple
fields are set.
</td>
</tr>
<tr>
<td>
<code>Object</code></br>
<em>
k8s.io/apimachinery/pkg/runtime.Object
</em>
</td>
<td>
</td>
</tr>
<tr>
<td>
<code>BuildSpec</code></br>
<em>
github.com/knative/build/pkg/apis/build/v1alpha1.BuildSpec
</em>
</td>
<td>
</td>
</tr>
</tbody>
</table>
<h3 id="ReleaseType">ReleaseType
</h3>
<p>
(<em>Appears on:</em>
<a href="#ServiceSpec">ServiceSpec</a>)
</p>
<p>
ReleaseType contains the options for slowly releasing revisions. See ServiceSpec for
more details.
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
<code>revisions</code></br>
<em>
[]string
</em>
</td>
<td>
<em>(Optional)</em>
Revisions is an ordered list of 1 or 2 revisions. The first will
have a TrafficTarget with a name of "current" and the second will have
a name of "candidate".
</td>
</tr>
<tr>
<td>
<code>rolloutPercent</code></br>
<em>
int
</em>
</td>
<td>
<em>(Optional)</em>
RolloutPercent is the percent of traffic that should be sent to the "candidate"
revision. Valid values are between 0 and 99 inclusive.
</td>
</tr>
<tr>
<td>
<code>configuration</code></br>
<em>
<a href="#ConfigurationSpec">
ConfigurationSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
The configuration for this service. All revisions from this service must
come from a single configuration.
</td>
</tr>
</tbody>
</table>
<h3 id="RevisionContainerConcurrencyType">RevisionContainerConcurrencyType
(<code>int64</code> alias)</p></h3>
<p>
(<em>Appears on:</em>
<a href="#PodAutoscalerSpec">PodAutoscalerSpec</a>,
<a href="#RevisionSpec">RevisionSpec</a>)
</p>
<p>
RevisionContainerConcurrencyType is an integer expressing a number of
in-flight (concurrent) requests.
</p>
<h3 id="RevisionRequestConcurrencyModelType">RevisionRequestConcurrencyModelType
(<code>string</code> alias)</p></h3>
<p>
(<em>Appears on:</em>
<a href="#RevisionSpec">RevisionSpec</a>,
<a href="#PodAutoscalerSpec">PodAutoscalerSpec</a>)
</p>
<p>
RevisionRequestConcurrencyModelType is an enumeration of the
concurrency models supported by a Revision.
Deprecated in favor of RevisionContainerConcurrencyType.
</p>
<h3 id="RevisionSpec">RevisionSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#Revision">Revision</a>,
<a href="#RevisionTemplateSpec">RevisionTemplateSpec</a>)
</p>
<p>
RevisionSpec holds the desired state of the Revision (from the client).
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
<code>servingState</code></br>
<em>
<a href="#DeprecatedRevisionServingStateType">
DeprecatedRevisionServingStateType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
DeprecatedServingState holds a value describing the desired state the Kubernetes
resources should be in for this Revision.
Users must not specify this when creating a revision. These values are no longer
updated by the system.
</td>
</tr>
<tr>
<td>
<code>concurrencyModel</code></br>
<em>
<a href="#RevisionRequestConcurrencyModelType">
RevisionRequestConcurrencyModelType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
ConcurrencyModel specifies the desired concurrency model
(Single or Multi) for the
Revision. Defaults to Multi.
Deprecated in favor of ContainerConcurrency.
</td>
</tr>
<tr>
<td>
<code>containerConcurrency</code></br>
<em>
<a href="#RevisionContainerConcurrencyType">
RevisionContainerConcurrencyType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
ContainerConcurrency specifies the maximum allowed
in-flight (concurrent) requests per container of the Revision.
Defaults to `0` which means unlimited concurrency.
This field replaces ConcurrencyModel. A value of `1`
is equivalent to `Single` and `0` is equivalent to `Multi`.
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
ServiceAccountName holds the name of the Kubernetes service account
as which the underlying K8s resources should be run. If unspecified
this will default to the "default" service account for the namespace
in which the Revision exists.
This may be used to provide access to private container images by
following: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/#add-imagepullsecrets-to-a-service-account
TODO(ZhiminXiang): verify the corresponding service account exists.
</td>
</tr>
<tr>
<td>
<code>buildName</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
BuildName optionally holds the name of the Build responsible for
producing the container image for its Revision.
DEPRECATED: Use BuildRef instead.
</td>
</tr>
<tr>
<td>
<code>buildRef</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectreference-v1-core">
Kubernetes core/v1.ObjectReference
</a>
</em>
</td>
<td>
<em>(Optional)</em>
BuildRef holds the reference to the build (if there is one) responsible
for producing the container image for this Revision. Otherwise, nil
</td>
</tr>
<tr>
<td>
<code>container</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#container-v1-core">
Kubernetes core/v1.Container
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Container defines the unit of execution for this Revision.
In the context of a Revision, we disallow a number of the fields of
this Container, including: name, resources, ports, and volumeMounts.
TODO(mattmoor): Link to the runtime contract tracked by:
https://github.com/knative/serving/issues/627
</td>
</tr>
<tr>
<td>
<code>timeoutSeconds</code></br>
<em>
<a href="https://godoc.org/k8s.io/apimachinery/pkg/apis/meta/v1#Duration">
Kubernetes meta/v1.Duration
</a>
</em>
</td>
<td>
<em>(Optional)</em>
TimeoutSeconds holds the max duration the instance is allowed for responding to a request.
</td>
</tr>
</tbody>
</table>
<h3 id="RevisionStatus">RevisionStatus
</h3>
<p>
(<em>Appears on:</em>
<a href="#Revision">Revision</a>)
</p>
<p>
RevisionStatus communicates the observed state of the Revision (from the controller).
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
<code>serviceName</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
ServiceName holds the name of a core Kubernetes Service resource that
load balances over the pods backing this Revision. When the Revision
is Active, this service would be an appropriate ingress target for
targeting the revision.
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
Conditions communicates information about ongoing/complete
reconciliation processes that bring the "spec" inline with the observed
state of the world.
</td>
</tr>
<tr>
<td>
<code>observedGeneration</code></br>
<em>
int64
</em>
</td>
<td>
<em>(Optional)</em>
ObservedGeneration is the 'Generation' of the Configuration that
was last processed by the controller. The observed generation is updated
even if the controller failed to process the spec and create the Revision.
</td>
</tr>
<tr>
<td>
<code>logUrl</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
LogURL specifies the generated logging url for this particular revision
based on the revision url template specified in the controller's config.
</td>
</tr>
<tr>
<td>
<code>imageDigest</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
ImageDigest holds the resolved digest for the image specified
within .Spec.Container.Image. The digest is resolved during the creation
of Revision. This field holds the digest value regardless of whether
a tag or digest was originally specified in the Container object. It
may be empty if the image comes from a registry listed to skip resolution.
</td>
</tr>
</tbody>
</table>
<h3 id="RevisionTemplateSpec">RevisionTemplateSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#ConfigurationSpec">ConfigurationSpec</a>)
</p>
<p>
RevisionTemplateSpec describes the data a revision should have when created from a template.
Based on: https://github.com/kubernetes/api/blob/e771f807/core/v1/types.go#L3179-L3190
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
<code>metadata</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectmeta-v1-meta">
Kubernetes meta/v1.ObjectMeta
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Refer to the Kubernetes API documentation for the fields of the
<code>metadata</code> field.
</td>
</tr>
<tr>
<td>
<code>spec</code></br>
<em>
<a href="#RevisionSpec">
RevisionSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
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
<code>servingState</code></br>
<em>
<a href="#DeprecatedRevisionServingStateType">
DeprecatedRevisionServingStateType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
DeprecatedServingState holds a value describing the desired state the Kubernetes
resources should be in for this Revision.
Users must not specify this when creating a revision. These values are no longer
updated by the system.
</td>
</tr>
<tr>
<td>
<code>concurrencyModel</code></br>
<em>
<a href="#RevisionRequestConcurrencyModelType">
RevisionRequestConcurrencyModelType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
ConcurrencyModel specifies the desired concurrency model
(Single or Multi) for the
Revision. Defaults to Multi.
Deprecated in favor of ContainerConcurrency.
</td>
</tr>
<tr>
<td>
<code>containerConcurrency</code></br>
<em>
<a href="#RevisionContainerConcurrencyType">
RevisionContainerConcurrencyType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
ContainerConcurrency specifies the maximum allowed
in-flight (concurrent) requests per container of the Revision.
Defaults to `0` which means unlimited concurrency.
This field replaces ConcurrencyModel. A value of `1`
is equivalent to `Single` and `0` is equivalent to `Multi`.
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
ServiceAccountName holds the name of the Kubernetes service account
as which the underlying K8s resources should be run. If unspecified
this will default to the "default" service account for the namespace
in which the Revision exists.
This may be used to provide access to private container images by
following: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/#add-imagepullsecrets-to-a-service-account
TODO(ZhiminXiang): verify the corresponding service account exists.
</td>
</tr>
<tr>
<td>
<code>buildName</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
BuildName optionally holds the name of the Build responsible for
producing the container image for its Revision.
DEPRECATED: Use BuildRef instead.
</td>
</tr>
<tr>
<td>
<code>buildRef</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#objectreference-v1-core">
Kubernetes core/v1.ObjectReference
</a>
</em>
</td>
<td>
<em>(Optional)</em>
BuildRef holds the reference to the build (if there is one) responsible
for producing the container image for this Revision. Otherwise, nil
</td>
</tr>
<tr>
<td>
<code>container</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#container-v1-core">
Kubernetes core/v1.Container
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Container defines the unit of execution for this Revision.
In the context of a Revision, we disallow a number of the fields of
this Container, including: name, resources, ports, and volumeMounts.
TODO(mattmoor): Link to the runtime contract tracked by:
https://github.com/knative/serving/issues/627
</td>
</tr>
<tr>
<td>
<code>timeoutSeconds</code></br>
<em>
<a href="https://godoc.org/k8s.io/apimachinery/pkg/apis/meta/v1#Duration">
Kubernetes meta/v1.Duration
</a>
</em>
</td>
<td>
<em>(Optional)</em>
TimeoutSeconds holds the max duration the instance is allowed for responding to a request.
</td>
</tr>
</table>
</td>
</tr>
</tbody>
</table>
<h3 id="RouteSpec">RouteSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#Route">Route</a>)
</p>
<p>
RouteSpec holds the desired state of the Route (from the client).
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
<code>traffic</code></br>
<em>
<a href="#TrafficTarget">
[]TrafficTarget
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Traffic specifies how to distribute traffic over a collection of Knative Serving Revisions and Configurations.
</td>
</tr>
</tbody>
</table>
<h3 id="RouteStatus">RouteStatus
</h3>
<p>
(<em>Appears on:</em>
<a href="#Route">Route</a>)
</p>
<p>
RouteStatus communicates the observed state of the Route (from the controller).
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
<code>domain</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
Domain holds the top-level domain that will distribute traffic over the provided targets.
It generally has the form {route-name}.{route-namespace}.{cluster-level-suffix}
</td>
</tr>
<tr>
<td>
<code>domainInternal</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
DomainInternal holds the top-level domain that will distribute traffic over the provided
targets from inside the cluster. It generally has the form
{route-name}.{route-namespace}.svc.cluster.local
DEPRECATED: Use Address instead.
</td>
</tr>
<tr>
<td>
<code>address</code></br>
<em>
<a href="https://godoc.org/github.com/knative/pkg/apis/duck/v1alpha1#Addressable">
github.com/knative/pkg/apis/duck/v1alpha1.Addressable
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Address holds the information needed for a Route to be the target of an event.
</td>
</tr>
<tr>
<td>
<code>traffic</code></br>
<em>
<a href="#TrafficTarget">
[]TrafficTarget
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Traffic holds the configured traffic distribution.
These entries will always contain RevisionName references.
When ConfigurationName appears in the spec, this will hold the
LatestReadyRevisionName that we last observed.
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
Conditions communicates information about ongoing/complete
reconciliation processes that bring the "spec" inline with the observed
state of the world.
</td>
</tr>
<tr>
<td>
<code>observedGeneration</code></br>
<em>
int64
</em>
</td>
<td>
<em>(Optional)</em>
ObservedGeneration is the 'Generation' of the Configuration that
was last processed by the controller. The observed generation is updated
even if the controller failed to process the spec and create the Revision.
</td>
</tr>
</tbody>
</table>
<h3 id="RunLatestType">RunLatestType
</h3>
<p>
(<em>Appears on:</em>
<a href="#ServiceSpec">ServiceSpec</a>)
</p>
<p>
RunLatestType contains the options for always having a route to the latest configuration. See
ServiceSpec for more details.
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
<code>configuration</code></br>
<em>
<a href="#ConfigurationSpec">
ConfigurationSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
The configuration for this service.
</td>
</tr>
</tbody>
</table>
<h3 id="ServiceSpec">ServiceSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#Service">Service</a>)
</p>
<p>
ServiceSpec represents the configuration for the Service object. Exactly one
of its members (other than Generation) must be specified. Services can either
track the latest ready revision of a configuration or be pinned to a specific
revision.
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
<code>runLatest</code></br>
<em>
<a href="#RunLatestType">
RunLatestType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
RunLatest defines a simple Service. It will automatically
configure a route that keeps the latest ready revision
from the supplied configuration running.
</td>
</tr>
<tr>
<td>
<code>pinned</code></br>
<em>
<a href="#PinnedType">
PinnedType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Pins this service to a specific revision name. The revision must
be owned by the configuration provided.
PinnedType is DEPRECATED in favor of ReleaseType
</td>
</tr>
<tr>
<td>
<code>manual</code></br>
<em>
<a href="#ManualType">
ManualType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Manual mode enables users to start managing the underlying Route and Configuration
resources directly.  This advanced usage is intended as a path for users to graduate
from the limited capabilities of Service to the full power of Route.
</td>
</tr>
<tr>
<td>
<code>release</code></br>
<em>
<a href="#ReleaseType">
ReleaseType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Release enables gradual promotion of new revisions by allowing traffic
to be split between two revisions. This type replaces the deprecated Pinned type.
</td>
</tr>
</tbody>
</table>
<h3 id="ServiceStatus">ServiceStatus
</h3>
<p>
(<em>Appears on:</em>
<a href="#Service">Service</a>)
</p>
<p>
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
</td>
</tr>
<tr>
<td>
<code>domain</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
From RouteStatus.
Domain holds the top-level domain that will distribute traffic over the provided targets.
It generally has the form {route-name}.{route-namespace}.{cluster-level-suffix}
</td>
</tr>
<tr>
<td>
<code>domainInternal</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
From RouteStatus.
DomainInternal holds the top-level domain that will distribute traffic over the provided
targets from inside the cluster. It generally has the form
{route-name}.{route-namespace}.svc.cluster.local
DEPRECATED: Use Address instead.
</td>
</tr>
<tr>
<td>
<code>address</code></br>
<em>
<a href="https://godoc.org/github.com/knative/pkg/apis/duck/v1alpha1#Addressable">
github.com/knative/pkg/apis/duck/v1alpha1.Addressable
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Address holds the information needed for a Route to be the target of an event.
</td>
</tr>
<tr>
<td>
<code>traffic</code></br>
<em>
<a href="#TrafficTarget">
[]TrafficTarget
</a>
</em>
</td>
<td>
<em>(Optional)</em>
From RouteStatus.
Traffic holds the configured traffic distribution.
These entries will always contain RevisionName references.
When ConfigurationName appears in the spec, this will hold the
LatestReadyRevisionName that we last observed.
</td>
</tr>
<tr>
<td>
<code>latestReadyRevisionName</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
From ConfigurationStatus.
LatestReadyRevisionName holds the name of the latest Revision stamped out
from this Service's Configuration that has had its "Ready" condition become "True".
</td>
</tr>
<tr>
<td>
<code>latestCreatedRevisionName</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
From ConfigurationStatus.
LatestCreatedRevisionName is the last revision that was created from this Service's
Configuration. It might not be ready yet, for that use LatestReadyRevisionName.
</td>
</tr>
<tr>
<td>
<code>observedGeneration</code></br>
<em>
int64
</em>
</td>
<td>
<em>(Optional)</em>
ObservedGeneration is the 'Generation' of the Service that
was last processed by the controller.
</td>
</tr>
</tbody>
</table>
<h3 id="TrafficTarget">TrafficTarget
</h3>
<p>
(<em>Appears on:</em>
<a href="#RouteSpec">RouteSpec</a>,
<a href="#RouteStatus">RouteStatus</a>,
<a href="#ServiceStatus">ServiceStatus</a>)
</p>
<p>
TrafficTarget holds a single entry of the routing table for a Route.
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
<em>(Optional)</em>
Name is optionally used to expose a dedicated hostname for referencing this
target exclusively. It has the form: {name}.${route.status.domain}
</td>
</tr>
<tr>
<td>
<code>revisionName</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
RevisionName of a specific revision to which to send this portion of traffic.
This is mutually exclusive with ConfigurationName.
</td>
</tr>
<tr>
<td>
<code>configurationName</code></br>
<em>
string
</em>
</td>
<td>
<em>(Optional)</em>
ConfigurationName of a configuration to whose latest revision we will send
this portion of traffic. When the "status.latestReadyRevisionName" of the
referenced configuration changes, we will automatically migrate traffic
from the prior "latest ready" revision to the new one.
This field is never set in Route's status, only its spec.
This is mutually exclusive with RevisionName.
</td>
</tr>
<tr>
<td>
<code>percent</code></br>
<em>
int
</em>
</td>
<td>
Percent specifies percent of the traffic to this Revision or Configuration.
This defaults to zero if unspecified.
</td>
</tr>
</tbody>
</table>
<hr/>
<h2>
autoscaling.internal.knative.dev
</h2>
<p>
</p>
Resource Types:
<ul><li>
<a href="#PodAutoscaler">PodAutoscaler</a>
</li></ul>
<h3 id="PodAutoscaler">PodAutoscaler
</h3>
<p>
PodAutoscaler is a Knative abstraction that encapsulates the interface by which Knative
components instantiate autoscalers.  This definition is an abstraction that may be backed
by multiple definitions.  For more information, see the Knative Pluggability presentation:
https://docs.google.com/presentation/d/10KWynvAJYuOEWy69VBa6bHJVCqIsz1TNdEKosNvcpPY/edit
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
autoscaling.knative.dev/v1alpha1
</code>
</td>
</tr>
<tr>
<td>
<code>kind</code></br>
string
</td>
<td><code>PodAutoscaler</code></td>
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
<em>(Optional)</em>
Refer to the Kubernetes API documentation for the fields of the
<code>metadata</code> field.
</td>
</tr>
<tr>
<td>
<code>spec</code></br>
<em>
<a href="#PodAutoscalerSpec">
PodAutoscalerSpec
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Spec holds the desired state of the PodAutoscaler (from the client).
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
<code>concurrencyModel</code></br>
<em>
<a href="#RevisionRequestConcurrencyModelType">
RevisionRequestConcurrencyModelType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
ConcurrencyModel specifies the desired concurrency model
(Single or Multi) for the scale target. Defaults to Multi.
Deprecated in favor of ContainerConcurrency.
</td>
</tr>
<tr>
<td>
<code>containerConcurrency</code></br>
<em>
<a href="#RevisionContainerConcurrencyType">
RevisionContainerConcurrencyType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
ContainerConcurrency specifies the maximum allowed
in-flight (concurrent) requests per container of the Revision.
Defaults to `0` which means unlimited concurrency.
This field replaces ConcurrencyModel. A value of `1`
is equivalent to `Single` and `0` is equivalent to `Multi`.
</td>
</tr>
<tr>
<td>
<code>scaleTargetRef</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#crossversionobjectreference-v1-autoscaling">
Kubernetes autoscaling/v1.CrossVersionObjectReference
</a>
</em>
</td>
<td>
ScaleTargetRef defines the /scale-able resource that this PodAutoscaler
is responsible for quickly right-sizing.
</td>
</tr>
<tr>
<td>
<code>serviceName</code></br>
<em>
string
</em>
</td>
<td>
ServiceName holds the name of a core Kubernetes Service resource that
load balances over the pods referenced by the ScaleTargetRef.
</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>
<code>status</code></br>
<em>
<a href="#PodAutoscalerStatus">
PodAutoscalerStatus
</a>
</em>
</td>
<td>
<em>(Optional)</em>
Status communicates the observed state of the PodAutoscaler (from the controller).
</td>
</tr>
</tbody>
</table>
<h3 id="PodAutoscalerSpec">PodAutoscalerSpec
</h3>
<p>
(<em>Appears on:</em>
<a href="#PodAutoscaler">PodAutoscaler</a>)
</p>
<p>
PodAutoscalerSpec holds the desired state of the PodAutoscaler (from the client).
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
<code>concurrencyModel</code></br>
<em>
<a href="#RevisionRequestConcurrencyModelType">
RevisionRequestConcurrencyModelType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
ConcurrencyModel specifies the desired concurrency model
(Single or Multi) for the scale target. Defaults to Multi.
Deprecated in favor of ContainerConcurrency.
</td>
</tr>
<tr>
<td>
<code>containerConcurrency</code></br>
<em>
<a href="#RevisionContainerConcurrencyType">
RevisionContainerConcurrencyType
</a>
</em>
</td>
<td>
<em>(Optional)</em>
ContainerConcurrency specifies the maximum allowed
in-flight (concurrent) requests per container of the Revision.
Defaults to `0` which means unlimited concurrency.
This field replaces ConcurrencyModel. A value of `1`
is equivalent to `Single` and `0` is equivalent to `Multi`.
</td>
</tr>
<tr>
<td>
<code>scaleTargetRef</code></br>
<em>
<a href="https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/#crossversionobjectreference-v1-autoscaling">
Kubernetes autoscaling/v1.CrossVersionObjectReference
</a>
</em>
</td>
<td>
ScaleTargetRef defines the /scale-able resource that this PodAutoscaler
is responsible for quickly right-sizing.
</td>
</tr>
<tr>
<td>
<code>serviceName</code></br>
<em>
string
</em>
</td>
<td>
ServiceName holds the name of a core Kubernetes Service resource that
load balances over the pods referenced by the ScaleTargetRef.
</td>
</tr>
</tbody>
</table>
<h3 id="PodAutoscalerStatus">PodAutoscalerStatus
</h3>
<p>
(<em>Appears on:</em>
<a href="#PodAutoscaler">PodAutoscaler</a>)
</p>
<p>
PodAutoscalerStatus communicates the observed state of the PodAutoscaler (from the controller).
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
Conditions communicates information about ongoing/complete
reconciliation processes that bring the "spec" inline with the observed
state of the world.
</td>
</tr>
</tbody>
</table>
<hr/>

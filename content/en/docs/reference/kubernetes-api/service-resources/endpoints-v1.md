---
api_metadata:
  apiVersion: "v1"
  import: "k8s.io/api/core/v1"
  kind: "Endpoints"
content_type: "api_reference"
description: "Endpoints is a collection of endpoints that implement the actual service."
title: "Endpoints"
weight: 2
auto_generated: true
---

<!--
The file is auto-generated from the Go source code of the component using a generic
[generator](https://github.com/kubernetes-sigs/reference-docs/). To learn how
to generate the reference documentation, please read
[Contributing to the reference documentation](/docs/contribute/generate-ref-docs/).
To update the reference content, please follow the 
[Contributing upstream](/docs/contribute/generate-ref-docs/contribute-upstream/)
guide. You can file document formatting bugs against the
[reference-docs](https://github.com/kubernetes-sigs/reference-docs/) project.
-->

`apiVersion: v1`

`import "k8s.io/api/core/v1"`


## Endpoints {#Endpoints}

Endpoints is a collection of endpoints that implement the actual service. Example:

   Name: "mysvc",
   Subsets: [
     {
       Addresses: [{"ip": "10.10.1.1"}, {"ip": "10.10.2.2"}],
       Ports: [{"name": "a", "port": 8675}, {"name": "b", "port": 309}]
     },
     {
       Addresses: [{"ip": "10.10.3.3"}],
       Ports: [{"name": "a", "port": 93}, {"name": "b", "port": 76}]
     },
  ]

Endpoints is a legacy API and does not contain information about all Service features. Use discoveryv1.EndpointSlice for complete information about Service endpoints.

Deprecated: This API is deprecated in v1.33+. Use discoveryv1.EndpointSlice.

<hr>

- **apiVersion**: v1


- **kind**: Endpoints


- **metadata** (<a href="{{< ref "../common-definitions/object-meta#ObjectMeta" >}}">ObjectMeta</a>)

  Standard object's metadata. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#metadata

- **subsets** ([]EndpointSubset)

  *Atomic: will be replaced during a merge*
  
  The set of all endpoints is the union of all subsets. Addresses are placed into subsets according to the IPs they share. A single address with multiple ports, some of which are ready and some of which are not (because they come from different containers) will result in the address being displayed in different subsets for the different ports. No address will appear in both Addresses and NotReadyAddresses in the same subset. Sets of addresses and ports that comprise a service.

  <a name="EndpointSubset"></a>
  *EndpointSubset is a group of addresses with a common set of ports. The expanded set of endpoints is the Cartesian product of Addresses x Ports. For example, given:
  
  	{
  	  Addresses: [{"ip": "10.10.1.1"}, {"ip": "10.10.2.2"}],
  	  Ports:     [{"name": "a", "port": 8675}, {"name": "b", "port": 309}]
  	}
  
  The resulting set of endpoints can be viewed as:
  
  	a: [ 10.10.1.1:8675, 10.10.2.2:8675 ],
  	b: [ 10.10.1.1:309, 10.10.2.2:309 ]
  
  Deprecated: This API is deprecated in v1.33+.*

  - **subsets.addresses** ([]EndpointAddress)

    *Atomic: will be replaced during a merge*
    
    IP addresses which offer the related ports that are marked as ready. These endpoints should be considered safe for load balancers and clients to utilize.

    <a name="EndpointAddress"></a>
    *EndpointAddress is a tuple that describes single IP address. Deprecated: This API is deprecated in v1.33+.*

    - **subsets.addresses.ip** (string), required

      The IP of this endpoint. May not be loopback (127.0.0.0/8 or ::1), link-local (169.254.0.0/16 or fe80::/10), or link-local multicast (224.0.0.0/24 or ff02::/16).

    - **subsets.addresses.hostname** (string)

      The Hostname of this endpoint

    - **subsets.addresses.nodeName** (string)

      Optional: Node hosting this endpoint. This can be used to determine endpoints local to a node.

    - **subsets.addresses.targetRef** (<a href="{{< ref "../common-definitions/object-reference#ObjectReference" >}}">ObjectReference</a>)

      Reference to object providing the endpoint.

  - **subsets.notReadyAddresses** ([]EndpointAddress)

    *Atomic: will be replaced during a merge*
    
    IP addresses which offer the related ports but are not currently marked as ready because they have not yet finished starting, have recently failed a readiness check, or have recently failed a liveness check.

    <a name="EndpointAddress"></a>
    *EndpointAddress is a tuple that describes single IP address. Deprecated: This API is deprecated in v1.33+.*

    - **subsets.notReadyAddresses.ip** (string), required

      The IP of this endpoint. May not be loopback (127.0.0.0/8 or ::1), link-local (169.254.0.0/16 or fe80::/10), or link-local multicast (224.0.0.0/24 or ff02::/16).

    - **subsets.notReadyAddresses.hostname** (string)

      The Hostname of this endpoint

    - **subsets.notReadyAddresses.nodeName** (string)

      Optional: Node hosting this endpoint. This can be used to determine endpoints local to a node.

    - **subsets.notReadyAddresses.targetRef** (<a href="{{< ref "../common-definitions/object-reference#ObjectReference" >}}">ObjectReference</a>)

      Reference to object providing the endpoint.

  - **subsets.ports** ([]EndpointPort)

    *Atomic: will be replaced during a merge*
    
    Port numbers available on the related IP addresses.

    <a name="EndpointPort"></a>
    *EndpointPort is a tuple that describes a single port. Deprecated: This API is deprecated in v1.33+.*

    - **subsets.ports.port** (int32), required

      The port number of the endpoint.

    - **subsets.ports.protocol** (string)

      The IP protocol for this port. Must be UDP, TCP, or SCTP. Default is TCP.

    - **subsets.ports.name** (string)

      The name of this port.  This must match the 'name' field in the corresponding ServicePort. Must be a DNS_LABEL. Optional only if one port is defined.

    - **subsets.ports.appProtocol** (string)

      The application protocol for this port. This is used as a hint for implementations to offer richer behavior for protocols that they understand. This field follows standard Kubernetes label syntax. Valid values are either:
      
      * Un-prefixed protocol names - reserved for IANA standard service names (as per RFC-6335 and https://www.iana.org/assignments/service-names).
      
      * Kubernetes-defined prefixed names:
        * 'kubernetes.io/h2c' - HTTP/2 prior knowledge over cleartext as described in https://www.rfc-editor.org/rfc/rfc9113.html#name-starting-http-2-with-prior-
        * 'kubernetes.io/ws'  - WebSocket over cleartext as described in https://www.rfc-editor.org/rfc/rfc6455
        * 'kubernetes.io/wss' - WebSocket over TLS as described in https://www.rfc-editor.org/rfc/rfc6455
      
      * Other protocols should use implementation-defined prefixed names such as mycompany.com/my-custom-protocol.





## EndpointsList {#EndpointsList}

EndpointsList is a list of endpoints. Deprecated: This API is deprecated in v1.33+.

<hr>

- **apiVersion**: v1


- **kind**: EndpointsList


- **metadata** (<a href="{{< ref "../common-definitions/list-meta#ListMeta" >}}">ListMeta</a>)

  Standard list metadata. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds

- **items** ([]<a href="{{< ref "../service-resources/endpoints-v1#Endpoints" >}}">Endpoints</a>), required

  List of endpoints.





## Operations {#Operations}



<hr>






### `get` read the specified Endpoints

#### HTTP Request

GET /api/v1/namespaces/{namespace}/endpoints/{name}

#### Parameters


- **name** (*in path*): string, required

  name of the Endpoints


- **namespace** (*in path*): string, required

  <a href="{{< ref "../common-parameters/common-parameters#namespace" >}}">namespace</a>


- **pretty** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#pretty" >}}">pretty</a>



#### Response


200 (<a href="{{< ref "../service-resources/endpoints-v1#Endpoints" >}}">Endpoints</a>): OK

401: Unauthorized


### `list` list or watch objects of kind Endpoints

#### HTTP Request

GET /api/v1/namespaces/{namespace}/endpoints

#### Parameters


- **namespace** (*in path*): string, required

  <a href="{{< ref "../common-parameters/common-parameters#namespace" >}}">namespace</a>


- **allowWatchBookmarks** (*in query*): boolean

  <a href="{{< ref "../common-parameters/common-parameters#allowWatchBookmarks" >}}">allowWatchBookmarks</a>


- **continue** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#continue" >}}">continue</a>


- **fieldSelector** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#fieldSelector" >}}">fieldSelector</a>


- **labelSelector** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#labelSelector" >}}">labelSelector</a>


- **limit** (*in query*): integer

  <a href="{{< ref "../common-parameters/common-parameters#limit" >}}">limit</a>


- **pretty** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#pretty" >}}">pretty</a>


- **resourceVersion** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#resourceVersion" >}}">resourceVersion</a>


- **resourceVersionMatch** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#resourceVersionMatch" >}}">resourceVersionMatch</a>


- **sendInitialEvents** (*in query*): boolean

  <a href="{{< ref "../common-parameters/common-parameters#sendInitialEvents" >}}">sendInitialEvents</a>


- **timeoutSeconds** (*in query*): integer

  <a href="{{< ref "../common-parameters/common-parameters#timeoutSeconds" >}}">timeoutSeconds</a>


- **watch** (*in query*): boolean

  <a href="{{< ref "../common-parameters/common-parameters#watch" >}}">watch</a>



#### Response


200 (<a href="{{< ref "../service-resources/endpoints-v1#EndpointsList" >}}">EndpointsList</a>): OK

401: Unauthorized


### `list` list or watch objects of kind Endpoints

#### HTTP Request

GET /api/v1/endpoints

#### Parameters


- **allowWatchBookmarks** (*in query*): boolean

  <a href="{{< ref "../common-parameters/common-parameters#allowWatchBookmarks" >}}">allowWatchBookmarks</a>


- **continue** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#continue" >}}">continue</a>


- **fieldSelector** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#fieldSelector" >}}">fieldSelector</a>


- **labelSelector** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#labelSelector" >}}">labelSelector</a>


- **limit** (*in query*): integer

  <a href="{{< ref "../common-parameters/common-parameters#limit" >}}">limit</a>


- **pretty** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#pretty" >}}">pretty</a>


- **resourceVersion** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#resourceVersion" >}}">resourceVersion</a>


- **resourceVersionMatch** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#resourceVersionMatch" >}}">resourceVersionMatch</a>


- **sendInitialEvents** (*in query*): boolean

  <a href="{{< ref "../common-parameters/common-parameters#sendInitialEvents" >}}">sendInitialEvents</a>


- **timeoutSeconds** (*in query*): integer

  <a href="{{< ref "../common-parameters/common-parameters#timeoutSeconds" >}}">timeoutSeconds</a>


- **watch** (*in query*): boolean

  <a href="{{< ref "../common-parameters/common-parameters#watch" >}}">watch</a>



#### Response


200 (<a href="{{< ref "../service-resources/endpoints-v1#EndpointsList" >}}">EndpointsList</a>): OK

401: Unauthorized


### `create` create Endpoints

#### HTTP Request

POST /api/v1/namespaces/{namespace}/endpoints

#### Parameters


- **namespace** (*in path*): string, required

  <a href="{{< ref "../common-parameters/common-parameters#namespace" >}}">namespace</a>


- **body**: <a href="{{< ref "../service-resources/endpoints-v1#Endpoints" >}}">Endpoints</a>, required

  


- **dryRun** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#dryRun" >}}">dryRun</a>


- **fieldManager** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#fieldManager" >}}">fieldManager</a>


- **fieldValidation** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#fieldValidation" >}}">fieldValidation</a>


- **pretty** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#pretty" >}}">pretty</a>



#### Response


200 (<a href="{{< ref "../service-resources/endpoints-v1#Endpoints" >}}">Endpoints</a>): OK

201 (<a href="{{< ref "../service-resources/endpoints-v1#Endpoints" >}}">Endpoints</a>): Created

202 (<a href="{{< ref "../service-resources/endpoints-v1#Endpoints" >}}">Endpoints</a>): Accepted

401: Unauthorized


### `update` replace the specified Endpoints

#### HTTP Request

PUT /api/v1/namespaces/{namespace}/endpoints/{name}

#### Parameters


- **name** (*in path*): string, required

  name of the Endpoints


- **namespace** (*in path*): string, required

  <a href="{{< ref "../common-parameters/common-parameters#namespace" >}}">namespace</a>


- **body**: <a href="{{< ref "../service-resources/endpoints-v1#Endpoints" >}}">Endpoints</a>, required

  


- **dryRun** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#dryRun" >}}">dryRun</a>


- **fieldManager** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#fieldManager" >}}">fieldManager</a>


- **fieldValidation** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#fieldValidation" >}}">fieldValidation</a>


- **pretty** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#pretty" >}}">pretty</a>



#### Response


200 (<a href="{{< ref "../service-resources/endpoints-v1#Endpoints" >}}">Endpoints</a>): OK

201 (<a href="{{< ref "../service-resources/endpoints-v1#Endpoints" >}}">Endpoints</a>): Created

401: Unauthorized


### `patch` partially update the specified Endpoints

#### HTTP Request

PATCH /api/v1/namespaces/{namespace}/endpoints/{name}

#### Parameters


- **name** (*in path*): string, required

  name of the Endpoints


- **namespace** (*in path*): string, required

  <a href="{{< ref "../common-parameters/common-parameters#namespace" >}}">namespace</a>


- **body**: <a href="{{< ref "../common-definitions/patch#Patch" >}}">Patch</a>, required

  


- **dryRun** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#dryRun" >}}">dryRun</a>


- **fieldManager** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#fieldManager" >}}">fieldManager</a>


- **fieldValidation** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#fieldValidation" >}}">fieldValidation</a>


- **force** (*in query*): boolean

  <a href="{{< ref "../common-parameters/common-parameters#force" >}}">force</a>


- **pretty** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#pretty" >}}">pretty</a>



#### Response


200 (<a href="{{< ref "../service-resources/endpoints-v1#Endpoints" >}}">Endpoints</a>): OK

201 (<a href="{{< ref "../service-resources/endpoints-v1#Endpoints" >}}">Endpoints</a>): Created

401: Unauthorized


### `delete` delete Endpoints

#### HTTP Request

DELETE /api/v1/namespaces/{namespace}/endpoints/{name}

#### Parameters


- **name** (*in path*): string, required

  name of the Endpoints


- **namespace** (*in path*): string, required

  <a href="{{< ref "../common-parameters/common-parameters#namespace" >}}">namespace</a>


- **body**: <a href="{{< ref "../common-definitions/delete-options#DeleteOptions" >}}">DeleteOptions</a>

  


- **dryRun** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#dryRun" >}}">dryRun</a>


- **gracePeriodSeconds** (*in query*): integer

  <a href="{{< ref "../common-parameters/common-parameters#gracePeriodSeconds" >}}">gracePeriodSeconds</a>


- **ignoreStoreReadErrorWithClusterBreakingPotential** (*in query*): boolean

  <a href="{{< ref "../common-parameters/common-parameters#ignoreStoreReadErrorWithClusterBreakingPotential" >}}">ignoreStoreReadErrorWithClusterBreakingPotential</a>


- **pretty** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#pretty" >}}">pretty</a>


- **propagationPolicy** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#propagationPolicy" >}}">propagationPolicy</a>



#### Response


200 (<a href="{{< ref "../common-definitions/status#Status" >}}">Status</a>): OK

202 (<a href="{{< ref "../common-definitions/status#Status" >}}">Status</a>): Accepted

401: Unauthorized


### `deletecollection` delete collection of Endpoints

#### HTTP Request

DELETE /api/v1/namespaces/{namespace}/endpoints

#### Parameters


- **namespace** (*in path*): string, required

  <a href="{{< ref "../common-parameters/common-parameters#namespace" >}}">namespace</a>


- **body**: <a href="{{< ref "../common-definitions/delete-options#DeleteOptions" >}}">DeleteOptions</a>

  


- **continue** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#continue" >}}">continue</a>


- **dryRun** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#dryRun" >}}">dryRun</a>


- **fieldSelector** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#fieldSelector" >}}">fieldSelector</a>


- **gracePeriodSeconds** (*in query*): integer

  <a href="{{< ref "../common-parameters/common-parameters#gracePeriodSeconds" >}}">gracePeriodSeconds</a>


- **ignoreStoreReadErrorWithClusterBreakingPotential** (*in query*): boolean

  <a href="{{< ref "../common-parameters/common-parameters#ignoreStoreReadErrorWithClusterBreakingPotential" >}}">ignoreStoreReadErrorWithClusterBreakingPotential</a>


- **labelSelector** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#labelSelector" >}}">labelSelector</a>


- **limit** (*in query*): integer

  <a href="{{< ref "../common-parameters/common-parameters#limit" >}}">limit</a>


- **pretty** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#pretty" >}}">pretty</a>


- **propagationPolicy** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#propagationPolicy" >}}">propagationPolicy</a>


- **resourceVersion** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#resourceVersion" >}}">resourceVersion</a>


- **resourceVersionMatch** (*in query*): string

  <a href="{{< ref "../common-parameters/common-parameters#resourceVersionMatch" >}}">resourceVersionMatch</a>


- **sendInitialEvents** (*in query*): boolean

  <a href="{{< ref "../common-parameters/common-parameters#sendInitialEvents" >}}">sendInitialEvents</a>


- **timeoutSeconds** (*in query*): integer

  <a href="{{< ref "../common-parameters/common-parameters#timeoutSeconds" >}}">timeoutSeconds</a>



#### Response


200 (<a href="{{< ref "../common-definitions/status#Status" >}}">Status</a>): OK

401: Unauthorized


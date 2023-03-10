Resource Quotas:
================
When several users or teams share a cluster with a fixed number of nodes, there is a concern that one team could use more than its fair share of resources.

Resource quotas are a tool for administrators to address this concern

A resource quota, defined by a ResourceQuota object, provides constraints that limit aggregate resource consumption per namespace. 

It can limit the quantity of objects that can be created in a namespace by type, as well as the total amount of compute resources that may be consumed by resources in that project

Resource quotas work like this:
===============================	
	The administrator creates one ResourceQuota for each namespace.
	
	Users create resources (pods, services, etc.) in the namespace, and the quota system tracks usage to ensure it does not exceed hard resource limits defined in a ResourceQuota.
	
	If creating or updating a resource violates a quota constraint, the request will fail with HTTP status code 403 FORBIDDEN with a message explaining the constraint that would have been violated.
	
	If quota is enabled in a namespace for compute resources like cpu and memory, users must specify requests or limits for those values; otherwise, the quota system may reject pod creation.

Examples of policies that could be created using namespaces and quotas are:
===========================================================================
	In a cluster with a capacity of 32 GiB RAM, and 16 cores, let team A use 20 GiB and 10 cores, let B use 10GiB and 4 cores, and hold 2GiB and 2 cores in reserve for future allocation.
	
	Limit the “testing” namespace to using 1 core and 1GiB RAM. Let the “production” namespace use any amount.

	In case where the total capacity of the cluster is less than the sum of the quotas of the namespaces, there may be contention for resources. This is handled on a first-come-first-served basis



Compute Resource Quota:
=======================
we can limit the total sum of compute resources that can be requested in a given namespace

	limits.cpu	    # Across all pods in a non-terminal state, the sum of CPU limits cannot exceed this value.
	limits.memory	# Across all pods in a non-terminal state, the sum of memory limits cannot exceed this value.
	requests.cpu	# Across all pods in a non-terminal state, the sum of CPU requests cannot exceed this value.
	requests.memory	# Across all pods in a non-terminal state, the sum of memory requests cannot exceed this value.

Object Count Quota:
====================
limit the total number of count for each resource that can be created in a given namespace

	configmaps	            # The total number of config maps that can exist in the namespace.
	persistentvolumeclaims	# The total number of persistent volume claims that can exist in the namespace.
	pods					# The total number of pods in a non-terminal state that can exist in the namespace. A pod is in a terminal state if .status.phase in (Failed, Succeeded) is true.
	replicationcontrollers	# The total number of replication controllers that can exist in the namespace.
	resourcequotas			# The total number of resource quotas that can exist in the namespace.
	services				# The total number of services that can exist in the namespace.
	services.loadbalancers	# The total number of services of type load balancer that can exist in the namespace.
	services.nodeports		# The total number of services of type node port that can exist in the namespace.
	secrets					# The total number of secrets that can exist in the namespace.

Requests vs Limits:
===================
When allocating compute resources, each container may specify a request and a limit value for either CPU or memory. The quota can be configured to quota either value.

If the quota has a value specified for requests.cpu or requests.memory, then it requires that every incoming container makes an explicit request for those resources. If the quota has a value specified for limits.cpu or limits.memory, then it requires that every incoming container specifies an explicit limit for those resources.



More Info:
https://kubernetes.io/docs/concepts/policy/resource-quotas/#viewing-and-setting-quotas

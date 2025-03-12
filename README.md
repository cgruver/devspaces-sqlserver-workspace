# devspaces-sqlserver-workspace
Running MS SQL Server in an OpenShift Dev Spaces workspace

```bash
cat << EOF | oc apply -f -
apiVersion: security.openshift.io/v1
kind: SecurityContextConstraints
metadata:
  name: custom-devspaces-scc
allowHostDirVolumePlugin: false
allowHostIPC: false
allowHostNetwork: false
allowHostPID: false
allowHostPorts: false
allowPrivilegeEscalation: true
allowPrivilegedContainer: false
allowedCapabilities:
- SETUID
- SETGID
- NET_BIND_SERVICE
fsGroup:
  type: MustRunAs
groups: []
readOnlyRootFilesystem: false
runAsUser:
  type: MustRunAsRange
seLinuxContext:
  type: MustRunAs
supplementalGroups:
  type: RunAsAny
users: []
volumes:
- configMap
- downwardAPI
- emptyDir
- persistentVolumeClaim
- projected
- secret
EOF
```

```yaml
apiVersion: org.eclipse.che/v2 
kind: CheCluster   
spec:                         
  devEnvironments:       
    containerBuildConfiguration:
      openShiftSecurityContextConstraint: custom-devspaces-scc
    disableContainerBuildCapabilities: false
```

# Pod

| Group | Version | Kind |
| --- | --- | --- |
| Core | v1 | Pod |

Pod is a collection of containers that can run on a host. This resource is created by clients and scheduled onto hosts.

## METHOD
- [Create](create.md)
- [List](list.md)
- [Get](get.md)
- [Log](log.md)
- [Exec](exec.md)
- [Delete](delete.md)

## Reference:

### Pod

- [Pod/v1 core](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#pod-v1-core)

### PodSpec

- [PodSpec/v1 core](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#podspec-v1-core)

**PodSpec Properties**:

- **Supported**
  - **activeDeadlineSeconds** integer
  - **affinity** Affinity
  - **containers** Container array
  - **hostAliases** HostAlias array
  - **hostname** string
  - **imagePullSecrets** LocalObjectReference array
  - **initContainers** Container array
  - **nodeSelector** object
  - **restartPolicy** string
  - **securityContext** PodSecurityContext
    - **runAsUser** integer
    - **supplementalGroups** integer array
    - ~~seLinuxOptions~~ SELinuxOptions
    - ~~fsGroup~~ integer
    - ~~runAsNonRoot~~ boolean
  - **subdomain** string
  - **terminationGracePeriodSeconds** integer
  - **volumes** Volume array

- **Not Support**
  - ~~automountServiceAccountToken~~ boolean
  - ~~deprecatedServiceAccount~~ string
  - ~~dnsPolicy~~ string
  - ~~dnsConfig~~ PodDNSConfig
  - ~~hostNetwork~~ boolean
  - ~~hostPID~~ boolean
  - ~~hostIPC~~ boolean
  - ~~nodeName~~ string
  - ~~priorityClassName~~ string
  - ~~priority~~ integer
  - ~~schedulerName~~ string
  - ~~serviceAccountName~~ string
  - ~~tolerations~~ Toleration array

### Container

- [Container/v1 core](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#container-v1-core)

**Container Properties**:
- **Supported**
  - **args** string array
  - **command** string array
  - **env** EnvVar array
  - **envFrom** EnvFromSource array
  - **image** string
  - **imagePullPolicy** string
  - **lifecycle** Lifecycle
  - **livenessProbe** Probe
  - **name** string
  - **ports** ContainerPort array
  - **readinessProbe** Probe
  - **resources** ResourceRequirements
  - **securityContext** SecurityContext
    - **allowPrivilegeEscalation** boolean
    - **readOnlyRootFilesystem** boolean
    - **runAsNonRoot** boolean
    - ~~capabilities~~ Capabilities
    - ~~privileged~~ boolean
    - ~~runAsUser~~ integer
    - ~~seLinuxOptions~~ SELinuxOptions
  - **stdin** boolean
  - **stdinOnce** boolean
  - **terminationMessagePath** string
  - **terminationMessagePolicy** string
  - **tty** boolean
  - **volumeMounts** VolumeMount array
    - **name** string
    - **readOnly** boolean
    - **mountPath** string
    - **subPath** string
    - ~~mountPropagation~~ MountPropagationMode
  - **workingDir** string


### FlexVolumeSource

- [FlexVolumeSource/v1 core](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#flexvolumesource-v1-core)

**FlexVolumeSource Properties**:
- **Support**
  - options object
    - **volumeID** string (`volume name`)
  - readOnly boolean
  - secretRef LocalObjectReference
- **Not Support**
  - ~~driver~~ string
  - ~~fsType~~ string


### PodList

- [PodList/v1 core](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#podlist-v1-core)

### DeleteOptions

- [DeleteOptions/v1 meta](https://v1-9.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#deleteoptions-v1-meta)

### PodExecConfig

| Field | Type | Description |
| --- | --- | --- |
| tty | _bool_ | allocate a pseudo-TTY |
| attachStdin | _bool_ | Attach the standard input, makes possible user interaction |
| attachStderr | _bool_ | Attach the standard output |
| attachStdout | _bool_ | Attach the standard error |
| cmd | _string array_ | command to run |

### PodExecCreateResponse

| Field | Type | Description |
| --- | --- | --- |
| Id | _bool_ | id of exec process |

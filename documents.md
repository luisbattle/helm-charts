apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
  creationTimestamp: "2021-04-07T19:50:47Z"
  generation: 1
  labels:
    app: flux
    chart: flux-1.1.0
    heritage: Helm
    release: flux
  name: flux
  namespace: default
  resourceVersion: "56530101"
  selfLink: /apis/apps/v1/namespaces/default/deployments/flux
  uid: 8c34c73b-6ac1-4cad-ad5f-307f87195c1e
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: flux
      release: flux
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: flux
        release: flux
    spec:
      containers:
      - args:
        - --log-format=json
        - --ssh-keygen-dir=/var/fluxd/keygen
        - --k8s-secret-name=flux-git-deploy
        - --memcached-hostname=flux-memcached
        - --sync-state=secret
        - --memcached-service=
        - --git-url=git@bitbucket.org:aerolineasargentinas/ar-kubernetes.git
        - --git-branch=master
        - --git-path=system,releases/prod
        - --git-readonly=false
        - --git-user=FluxCD
        - --git-email=ar.devops@valtech.com
        - --git-verify-signatures=false
        - --git-set-author=false
        - --git-poll-interval=5m
        - --git-timeout=20s
        - --sync-interval=5m
        - --git-ci-skip=false
        - --git-label=flux-prod
        - --automation-interval=5m
        - --registry-rps=200
        - --registry-burst=125
        - --registry-trace=false
        - --connect=ws://fluxcloud.kube-system
        env:
        - name: KUBECONFIG
          value: /root/.kubectl/config
        image: docker.io/fluxcd/flux:1.17.1
        imagePullPolicy: IfNotPresent
        livenessProbe:
          failureThreshold: 3
          httpGet:
            path: /api/flux/v6/identity.pub
            port: 3030
            scheme: HTTP
          initialDelaySeconds: 5
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 5
        name: flux
        ports:
        - containerPort: 3030
          name: http
          protocol: TCP
        readinessProbe:
          failureThreshold: 3
          httpGet:
            path: /api/flux/v6/identity.pub
            port: 3030
            scheme: HTTP
          initialDelaySeconds: 5
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 5
        resources:
          requests:
            cpu: 50m
            memory: 64Mi
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
        volumeMounts:
        - mountPath: /root/.kubectl
          name: kubedir
        - mountPath: /etc/fluxd/ssh
          name: git-key
          readOnly: true
        - mountPath: /var/fluxd/keygen
          name: git-keygen
        - mountPath: /etc/kubernetes/azure.json
          name: acr-credentials
          readOnly: true
      dnsPolicy: ClusterFirst
      nodeSelector:
        beta.kubernetes.io/os: linux
        node-role.kubernetes.io/master: ""
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      serviceAccount: flux
      serviceAccountName: flux
      terminationGracePeriodSeconds: 30
      tolerations:
      - effect: NoSchedule
        key: node-role.kubernetes.io/master
      volumes:
      - configMap:
          defaultMode: 420
          name: flux-kube-config
        name: kubedir
      - name: git-key
        secret:
          defaultMode: 256
          secretName: flux-git-deploy
      - emptyDir:
          medium: Memory
        name: git-keygen
      - hostPath:
          path: /etc/kubernetes/azure.json
          type: ""
        name: acr-credentials
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: "2021-04-07T19:50:47Z"
    lastUpdateTime: "2021-04-07T19:51:57Z"
    message: ReplicaSet "flux-5c5d7c5f4c" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  - lastTransitionTime: "2021-10-05T06:40:21Z"
    lastUpdateTime: "2021-10-05T06:40:21Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  observedGeneration: 1
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1
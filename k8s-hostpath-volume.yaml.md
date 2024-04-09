volumeMounts:
    - name: hostpath-volume
      mountPath: /host-data
  volumes:
  - name: hostpath-volume
    hostPath:
      path: /var/data

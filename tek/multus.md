## kind networkattachmentdefinition

```
---
apiVersion: "k8s.cni.cncf.io/v1"
kind: NetworkAttachmentDefinition
metadata:
  name: sriov-net-a
  annotations:
    k8s.v1.cni.cncf.io/resourceName: intel.com/sriov
spec:
  config: '{
  "type": "sriov",
  "vlan": 1000,
  "ipam": {
    "type": "host-local",
    "subnet": "10.56.217.0/24",
    "rangeStart": "10.56.217.171",
    "rangeEnd": "10.56.217.181",
    "routes": [{
      "dst": "0.0.0.0/0"
    }],
    "gateway": "10.56.217.1"
  }
}'
```


- [kubernetes系列之十二：POD多网卡方案multus-cni之概览](https://blog.csdn.net/cloudvtech/article/details/80221988)
- [kubernetes系列之十三：POD多网卡方案multus-cni之通过CRD给POD配置自由组合网络](https://blog.csdn.net/cloudvtech/article/details/80238843)






/opt/gopath/src/github.com/NVIDIA/gpu-operator/controllers/clusterpolicy_controller.go

func (r *ClusterPolicyReconciler) Reconcile(ctx context.Context, req ctrl.Request) (ctrl.Result, error) {


NVIDIA GPU Operator有以下的组件构成：

安装nvidia driver的组件
安装nvidia container toolkit的组件
安装nvidia devcie plugin的组件
安装nvidia dcgm exporter组件
安装gpu feature discovery组件


- NVIDIA GPU Operator分析一：NVIDIA驱动安装 https://developer.aliyun.com/article/784149
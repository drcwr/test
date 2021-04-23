## 创建crd
demo dir

    /opt/gopath/src/github.com/drcwr/kubeopdemo

set module pattern

    export GO111MODULE=on

set go proxy,see [go-proxy.md](go-proxy.md)

    export GOPROXY=https://goproxy.cn

kubebuilder init

    /usr/local/bin/kubebuilder init --domain k8s.demo.com --license apache2 --owner "cwr" --skip-go-version-check

view file

    ls -R


create api

    kubebuilder create api --group groupa --version v1beta1 --kind ApiExampleA

modyfe Dockerfile

    RUN GOPROXY=https://gocenter.io  go mod download

build image

    make docker-build
    or
    sudo    make docker-build

deploy crd

    make install

get crd

    kubectl get crd

deploy deployment

    make deploy
    or
    ./bin/kustomize build config/default >kustomze1.yaml
    sudo kubectl delete -f kustomze1.yaml
    sudo kubectl apply -f kustomze1.yaml

create crd object

    kubectl apply -f config/samples/groupa_v1beta1_apiexamplea.yaml

get crd object

    kubectl get apiexamplea.groupa.k8s.demo.com
    kubectl get apiexamplea.groupa.k8s.demo.com apiexamplea-sample -o yaml

## 自定义逻辑

modefy types

    api/v1beta1/apiexamplea_types.go
    @@ -29,7 +29,9 @@ type ApiExampleASpec struct {
            // Important: Run "make" to regenerate code after modifying this file
     
            // Foo is an example field of ApiExampleA. Edit apiexamplea_types.go to remove/update
    -       Foo string `json:"foo,omitempty"`
    +       Foo     string `json:"foo,omitempty"`
    +       AppName string `json:"appName"`
    +       AppFunc string `json:"appFunc"`
     }



add controller logic

Makefile
    
    # Image URL to use all building/pushing image targets
    -IMG ?= controller:latest
    +IMG ?= controller:1



config/samples/groupa_v1beta1_apiexamplea.yaml

     spec:
       # Add fields here
       foo: bar
    +  appName: demo
    +  appFunc: run
    
controllers/apiexamplea_controller.go
```
 func (r *ApiExampleAReconciler) Reconcile(ctx context.Context, req ctrl.Request) (ctrl.Result, error) {
-       _ = r.Log.WithValues("apiexamplea", req.NamespacedName)
 
        // your logic here
+       ctx = context.Background()
+       _ = r.Log.WithValues("apiexamplea", req.NamespacedName)
 
+       obja := &groupav1beta1.ApiExampleA{}
+       if err := r.Get(ctx, req.NamespacedName, obja); err != nil {
+               log.Println(err, "nuable to fetch New Object")
+       } else {
+               log.Println("fetch New Object:", obja.Spec.AppName, obja.Spec.AppFunc)
+       }
        return ctrl.Result{}, nil
 }
```

update

    make & make install &make deploy
    kubectl apply -f config/samples/groupa_v1beta1_apiexamplea.yaml

    kubectl get pod -n kubeopdemo-system

log

    kubectl logs -n kubeopdemo-system   kubeopdemo-controller-manager-648b6674b-9rncm -c manager

    2021-04-20T06:53:15.794Z	INFO	controller-runtime.manager.controller.apiexamplea	Starting Controller	{"reconciler group": "groupa.k8s.demo.com", "reconciler kind": "ApiExampleA"}
    2021-04-20T06:53:15.794Z	INFO	controller-runtime.manager.controller.apiexamplea	Starting workers	{"reconciler group": "groupa.k8s.demo.com", "reconciler kind": "ApiExampleA", "worker count": 1}
    2021/04/20 07:35:32 fetch New Object: demo run
    
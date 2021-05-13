之前说到 kubebuilder 是开发 Operator 的框架，其实并不十分准确。准确来说，kubebuilder 是开发 Controller Manager 的框架，Controller Manager 会管理一个或者多个 Operator。因此 cmd 中的代码也主要是围绕 Controller Managter 展开的。manager.New 首先创建了一个 Manager 实例，在这一实例中有 client，cache 等之后需要的对象。

apis.AddToScheme 将 CRD 的结构与 Kubernetes GroupVersionKinds 的映射添加到 Manager 的 Scheme 中。

接下来，就是通过 controller.AddToManager 创建出定义的 Operator，并且添加到 Manager 中。这也就是前文提到的 add 函数做的事情。利用 controller.New 创建出 Operator，然后 Watch 对应的资源，最后返回。
![operator](files/01-k8s-operator.svg)

下面是 controller.New 的实现：
```
/opt/gopath/pkg/mod/sigs.k8s.io/controller-runtime@v0.7.2/pkg/controller/controller.go
// New returns a new Controller registered with the Manager.  The Manager will ensure that shared Caches have
// been synced before the Controller is Started.
func New(name string, mgr manager.Manager, options Options) (Controller, error) {
	c, err := NewUnmanaged(name, mgr, options)
	if err != nil {
		return nil, err
	}

	// Add the controller as a Manager components
	return c, mgr.Add(c)
}

// NewUnmanaged returns a new controller without adding it to the manager. The
// caller is responsible for starting the returned controller.
func NewUnmanaged(name string, mgr manager.Manager, options Options) (Controller, error) {
	if options.Reconciler == nil {
		return nil, fmt.Errorf("must specify Reconciler")
	}

	if len(name) == 0 {
		return nil, fmt.Errorf("must specify Name for Controller")
	}

	if options.Log == nil {
		options.Log = mgr.GetLogger()
	}

	if options.MaxConcurrentReconciles <= 0 {
		options.MaxConcurrentReconciles = 1
	}

	if options.RateLimiter == nil {
		options.RateLimiter = workqueue.DefaultControllerRateLimiter()
	}

	// Inject dependencies into Reconciler
	if err := mgr.SetFields(options.Reconciler); err != nil {
		return nil, err
	}

	// Create controller with dependencies set
	return &controller.Controller{
		Do: options.Reconciler,
		MakeQueue: func() workqueue.RateLimitingInterface {
			return workqueue.NewNamedRateLimitingQueue(options.RateLimiter, name)
		},
		MaxConcurrentReconciles: options.MaxConcurrentReconciles,
		SetFields:               mgr.SetFields,
		Name:                    name,
		Log:                     options.Log.WithName("controller").WithName(name),
	}, nil
}
```
其中 options.Reconciler 就是我们定义的实现了 Reconcile 函数的结构的实例。这一结构的 Reconcile 函数的实现也就是前文中提到的 Operator 实现所需的第二处需要修改的地方。mgr.SetFields(options.Reconciler) 利用依赖注入的方式，将 Manager 的 Client 和 Scheme 注入到 options.Reconciler 中，然后将其赋值给 Controller 中指向 reconcile.Reconciler 接口的字段 Do 中。可以看到除了这一字段，Controller 还有 Queue，Recorder， Client 等其他的字段。因此 kubebuilder 是对 Controller 进行了更高层次的抽象，其有关业务逻辑的实现都通过 reconcile.Reconciler 这一接口进行，而 Queue 等底层的对象，则是由 kubebuilder 来替开发者维护。

最后一步 mgr.Add(c) 将 Operator 加入到 Manager 的一个 Operator 数组中。
```
type controllerManager struct {
    // ...

	// runnables is the set of Controllers that the controllerManager injects deps into and Starts.
	runnables []Runnable
    // ...
}

// Add sets dependencies on i, and adds it to the list of runnables to start.
func (cm *controllerManager) Add(r Runnable) error {
	cm.mu.Lock()
	defer cm.mu.Unlock()

	// Set dependencies on the object
	if err := cm.SetFields(r); err != nil {
		return err
	}

	// Add the runnable to the list
	cm.runnables = append(cm.runnables, r)
	if cm.started {
		// If already started, start the controller
		go func() {
			cm.errChan <- r.Start(cm.internalStop)
		}()
	}

	return nil
}
```
接下来，我们回过头来看下 Controller 是如何实现 Watch Kubernetes API Server 的资源的：
```
// Watch implements controller.Controller
func (c *Controller) Watch(src source.Source, evthdler handler.EventHandler, prct ...predicate.Predicate) error {
	c.mu.Lock()
	defer c.mu.Unlock()

	// Inject Cache into arguments
	if err := c.SetFields(src); err != nil {
		return err
	}
	if err := c.SetFields(evthdler); err != nil {
		return err
	}
	for _, pr := range prct {
		if err := c.SetFields(pr); err != nil {
			return err
		}
	}

	log.Info("Starting EventSource", "controller", c.Name, "source", src)
	return src.Start(evthdler, c.Queue, prct...)
}
```
这里的 SetFields 是 Manager 将其自己的 SetFields 函数注入了进来，所以等同于调用了 Manager 的 SetFields 方法，其定义如下：

```
func (cm *controllerManager) SetFields(i interface{}) error {
	if _, err := inject.ConfigInto(cm.config, i); err != nil {
		return err
	}
	if _, err := inject.ClientInto(cm.client, i); err != nil {
		return err
	}
	if _, err := inject.SchemeInto(cm.scheme, i); err != nil {
		return err
	}
	if _, err := inject.CacheInto(cm.cache, i); err != nil {
		return err
	}
	if _, err := inject.InjectorInto(cm.SetFields, i); err != nil {
		return err
	}
	if _, err := inject.StopChannelInto(cm.internalStop, i); err != nil {
		return err
	}
	if _, err := inject.DecoderInto(cm.admissionDecoder, i); err != nil {
		return err
	}
	return nil
}
// Cache is used by the ControllerManager to inject Cache into Sources, EventHandlers, Predicates, and
// Reconciles
type Cache interface {
	InjectCache(cache cache.Cache) error
}
// CacheInto will set informers on i and return the result if it implements Cache.  Returns
//// false if i does not implement Cache.
func CacheInto(c cache.Cache, i interface{}) (bool, error) {
	if s, ok := i.(Cache); ok {
		return true, s.InjectCache(c)
	}
	return false, nil
}
```
以 inject.CacheInto 为例介绍下实现，其检查被注入的对象有没有实现 Cache 接口，即有没有实现 InjectCache(cache cache.Cache) error方法。如果实现了，则执行注入，否则直接返回。也就是通过这样的方式，Manager 最终把 Cache 注入到了 Source 中，同时如果需要的话，把 Scheme 注入到了 EventHandler 中。这里的 Scheme 在指定 Owner 的 EventHandler 会被用来获取 Owner 的 GroupKind。

接下来，后面 src.Start(evthdler, c.Queue, prct...) 也就是顺理成章的实现了：

```
// Start is internal and should be called only by the Controller to register an EventHandler with the Informer
// to enqueue reconcile.Requests.
func (ks *Kind) Start(handler handler.EventHandler, queue workqueue.RateLimitingInterface,
	prct ...predicate.Predicate) error {

	// Type should have been specified by the user.
	if ks.Type == nil {
		return fmt.Errorf("must specify Kind.Type")
	}

	// cache should have been injected before Start was called
	if ks.cache == nil {
		return fmt.Errorf("must call CacheInto on Kind before calling Start")
	}

	// Lookup the Informer from the Cache and add an EventHandler which populates the Queue
	i, err := ks.cache.GetInformer(ks.Type)
	if err != nil {
		if kindMatchErr, ok := err.(*meta.NoKindMatchError); ok {
			log.Error(err, "if kind is a CRD, it should be installed before calling Start",
				"kind", kindMatchErr.GroupKind)
		}
		return err
	}
	i.AddEventHandler(internal.EventHandler{Queue: queue, EventHandler: handler, Predicates: prct})
	return nil
}
```
其利用被注入到 Source 中的 Cache 获取到针对 Source 的资源类型的 Informer，然后将 EventHandler 作为处理 Informer 的事件的处理器。这就是 kubebuilder 的高层 API 背后做的事情。


- 参考
http://gaocegege.com/Blog/kubernetes/kubebuilder

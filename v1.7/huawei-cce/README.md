## To reproduce:

After setting up a cluster on Huawei Cloud, we start `E2E Conformance Tests` on one cluster node with following step:

### Do Some Env Setting Before Tests 

Because interface `kubectl` under `/home/linux` directory, we export it to `PATH`.

 ```shell
 cd /home/linux
 export PATH=$PATH:$(pwd)
 ```
 
 then, you can use `kubectl` anywhere.
 
### Launch E2E Conformance Tests

1. Launch e2e pod and sonobuoy master under namespace `sonobuoy`.

 ```shell
 curl -L https://raw.githubusercontent.com/cncf/k8s-conformance/master/sonobuoy-conformance-1.7.yaml | kubectl apply -f -
 ```
 
2. Check logs of `sonobuoy` pod to see when test can be finished.
 
 ```shell
 kubectl logs -f -n sonobuoy sonobuoy
 ```
 
 waitting for line `no-exit was specified, sonobuoy is now blocking` now.
 
3. Use `kubectl cp` to bring the results to local machine.

 ```shell
 kubectl cp sonobuoy/sonobuoy:/tmp/sonobuoy /home/result
 ```
 
4. Delete the conformance test resources.
 
 ```shell
 curl -L https://raw.githubusercontent.com/cncf/k8s-conformance/master/sonobuoy-conformance-1.7.yaml | kubectl delete -f -
 ```
 
5. Untar the tarball, then add plugins/e2e/results/{e2e.log,junit_01.xml}.
 
 ```shell
 cd /home/result
 tar -xzf *_sonobuoy_*.tar.gz
 ```
 
 Now, you get e2e result logs and others relative output files.
 
 ### Results Explain
 
 For some specially reason, you can also get one failure in logs, but just one:
 
 `[Fail] [k8s.io] Networking [BeforeEach] should provide Internet connection for containers [Conformance]`
 
 The failed reason can refer to [here](https://github.com/cncf/k8s-conformance/issues/27).
 

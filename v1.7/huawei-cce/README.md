## To reproduce:

After setting up a cluster on Huawei Cloud, we start `E2E Conformance Tests` on one cluster node with following step:

### Do Some Env Setting Before Tests 

 ```console
 $ cd /home/linux
 $ export PATH=$PATH:$(pwd)
 ```
 then, you can use `kubectl` anywhere.
 
### Launch E2E Conformance Tests

1) Launch e2e pod and sonobuoy master suggested in [https://github.com/cncf/k8s-conformance/pull/24](https://github.com/cncf/k8s-conformance/pull/24) under namespace `heptio-sonobuoy`.

 ```console
 $ curl -L https://raw.githubusercontent.com/heptio/sonobuoy/latest/examples/conformance.yaml | kubectl apply -f -
 ```
 
2) Check logs of `sonobuoy` pod to see when test can be finished.
 
 ```console
 $ kubectl logs -f -n heptio-sonobuoy sonobuoy
 ```
 waitting for `no-exit was specified, sonobuoy is now blocking` now.
 
3) Use `kubectl cp` to bring the results to local machine.

 ```console
 $ kubectl cp heptio-sonobuoy/sonobuoy:/tmp/sonobuoy /home/result
 ```
 
4) Delete the conformance test resources.
 
 ```console
 $ curl -L https://raw.githubusercontent.com/heptio/sonobuoy/latest/examples/conformance.yaml | kubectl delete -f -
 ```
 
5) Untar the tarball, then add plugins/e2e/results/{e2e.log,junit_01.xml}.
 
 ```console
 $ cd /home/result
 $ tar -xzf *_sonobuoy_*.tar.gz
 ```
 
 Now, you get e2e result logs and others relative files.
 
 #### Conformance Test SUCCESS!
 
 
 

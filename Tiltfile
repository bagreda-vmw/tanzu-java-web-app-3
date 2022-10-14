SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='709231817767.dkr.ecr.ap-southeast-1.amazonaws.com/22039580-4176-11ed-9e20-0202f20c5b5a/tap-supply-chain/tanzu-java-web-app-3-tap-workload')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='tap-workload')
allow_k8s_contexts('aks-tap13')
k8s_custom_deploy(
    'tanzu-java-web-app-3',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null" +
               " && kubectl get workload tanzu-java-web-app-3 --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('tanzu-java-web-app-3', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'tanzu-java-web-app-3'}])

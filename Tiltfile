SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='dev.local/tap-java-demo-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')
OUTPUT_TO_NULL_COMMAND = os.getenv("OUTPUT_TO_NULL_COMMAND", default=' > /dev/null ')
REGISTRY_USERNAME="504e3da4cc8e47269f70eba30ab4c61f"
REGISTRY_PASSWORD="JBUa2UNsITZiQyVQtajIEW4TAfn5c6IxQc4pnMHWEk+ACRCro02f"

k8s_custom_deploy(
    'tap-java-demo',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --update-strategy replace --debug --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --registry-username " + REGISTRY_USERNAME +
               " --registry-password " + REGISTRY_PASSWORD +
               " --namespace " + NAMESPACE +
               " --yes " +
               OUTPUT_TO_NULL_COMMAND +
               " && kubectl get workload tap-java-demo --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('tap-java-demo', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'carto.run/workload-name': 'tap-java-demo', 'app.kubernetes.io/component': 'run'}])

allow_k8s_contexts('tap-grey-iterate')

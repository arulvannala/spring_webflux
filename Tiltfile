#SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='dev.registry.pivotal.io/se-americas-west/tanzu-java-web-app')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')

SOURCE_IMAGE= 'dev.registry.pivotal.io/se-americas-west/math-demo-live'
#LOCAL_PATH = '/Users/arulvannala/Documents/workspace/tap/apps/math-demo-live'

k8s_custom_deploy(
    'math-demo-live',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null" +
               " && kubectl get workload math-demo-live --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

update_settings ( max_parallel_updates = 3 , k8s_upsert_timeout_secs = 300 , suppress_unused_image_warnings = None ) 

k8s_resource('math-demo-live', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'math-demo-live'}])

allow_k8s_contexts('gke_pa-avannala2_us-central1_tap-gke')

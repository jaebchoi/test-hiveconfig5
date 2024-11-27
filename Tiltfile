allow_k8s_contexts('local')
docker_prune_settings(num_builds=1, keep_recent=1)

aissemble_version = '1.11.0-SNAPSHOT'

build_args = { 'DOCKER_BASELINE_REPO_ID': 'ghcr.io/',
               'VERSION_AISSEMBLE': aissemble_version}

# Kafka
yaml = helm(
    'test-hiveconfig5-deploy/src/main/resources/apps/kafka-cluster',
    values=['test-hiveconfig5-deploy/src/main/resources/apps/kafka-cluster/values.yaml',
        'test-hiveconfig5-deploy/src/main/resources/apps/kafka-cluster/values-dev.yaml']
)
k8s_yaml(yaml)

# Add deployment resources here


# spark-worker-image
docker_build(
    ref='test-hiveconfig5-spark-worker-docker',
    context='test-hiveconfig5-docker/test-hiveconfig5-spark-worker-docker',
    build_args=build_args,
    extra_tag='test-hiveconfig5-spark-worker-docker:latest',
    dockerfile='test-hiveconfig5-docker/test-hiveconfig5-spark-worker-docker/src/main/resources/docker/Dockerfile'
)


k8s_kind('SparkApplication', image_json_path='{.spec.image}')

# policy-decision-point
docker_build(
    ref='test-hiveconfig5-policy-decision-point-docker',
    context='test-hiveconfig5-docker/test-hiveconfig5-policy-decision-point-docker',
    build_args=build_args,
    dockerfile='test-hiveconfig5-docker/test-hiveconfig5-policy-decision-point-docker/src/main/resources/docker/Dockerfile'
)



yaml = local('helm template oci://ghcr.io/boozallen/aissemble-spark-application-chart --version %s --values test-hiveconfig5-pipelines/spark-pipeline/src/main/resources/apps/spark-pipeline-base-values.yaml,test-hiveconfig5-pipelines/spark-pipeline/src/main/resources/apps/spark-pipeline-dev-values.yaml' % aissemble_version)
k8s_yaml(yaml)
k8s_resource('spark-pipeline', port_forwards=[port_forward(4747, 4747, 'debug')], auto_init=False, trigger_mode=TRIGGER_MODE_MANUAL)


yaml = helm(
   'test-hiveconfig5-deploy/src/main/resources/apps/spark-operator',
   name='spark-operator',
   values=['test-hiveconfig5-deploy/src/main/resources/apps/spark-operator/values.yaml',
       'test-hiveconfig5-deploy/src/main/resources/apps/spark-operator/values-dev.yaml']
)
k8s_yaml(yaml)
k8s_yaml('test-hiveconfig5-deploy/src/main/resources/apps/spark-worker-image/spark-worker-image.yaml')


yaml = helm(
   'test-hiveconfig5-deploy/src/main/resources/apps/spark-infrastructure',
   name='spark-infrastructure',
   values=['test-hiveconfig5-deploy/src/main/resources/apps/spark-infrastructure/values.yaml',
       'test-hiveconfig5-deploy/src/main/resources/apps/spark-infrastructure/values-dev.yaml']
)
k8s_yaml(yaml)
yaml = helm(
   'test-hiveconfig5-deploy/src/main/resources/apps/metadata',
   name='metadata',
   values=['test-hiveconfig5-deploy/src/main/resources/apps/metadata/values.yaml',
       'test-hiveconfig5-deploy/src/main/resources/apps/metadata/values-dev.yaml']
)
k8s_yaml(yaml)
yaml = helm(
   'test-hiveconfig5-deploy/src/main/resources/apps/policy-decision-point',
   name='policy-decision-point',
   values=['test-hiveconfig5-deploy/src/main/resources/apps/policy-decision-point/values.yaml',
       'test-hiveconfig5-deploy/src/main/resources/apps/policy-decision-point/values-dev.yaml']
)
k8s_yaml(yaml)

yaml = helm(
   'test-hiveconfig5-deploy/src/main/resources/apps/pipeline-invocation-service',
   name='pipeline-invocation-service',
   values=['test-hiveconfig5-deploy/src/main/resources/apps/pipeline-invocation-service/values.yaml',
       'test-hiveconfig5-deploy/src/main/resources/apps/pipeline-invocation-service/values-dev.yaml']
)
k8s_yaml(yaml)
yaml = helm(
   'test-hiveconfig5-deploy/src/main/resources/apps/s3-local',
   name='s3-local',
   values=['test-hiveconfig5-deploy/src/main/resources/apps/s3-local/values.yaml',
       'test-hiveconfig5-deploy/src/main/resources/apps/s3-local/values-dev.yaml']
)
k8s_yaml(yaml)

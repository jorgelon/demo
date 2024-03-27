# rollout

apiVersion: argoproj.io/v1alpha1
kind: Rollout
  strategy:
    blueGreen:
      activeService: web
      prePromotionAnalysis:
        templates:
        - templateName: success-rate
        args:
        - name: service-name
          value: guestbook-svc.default.svc.cluster.local
      postPromotionAnalysis:
        templates:
        - templateName: success-rate
        args:
        - name: service-name
          value: guestbook-svc.default.svc.cluster.local
      previewService: preview-service
      previewReplicaCount: 1
      autoPromotionEnabled: false
      autoPromotionSeconds: 30
      scaleDownDelaySeconds: 30
      scaleDownDelayRevisionLimit: 2
      abortScaleDownDelaySeconds: 30
      antiAffinity:
        requiredDuringSchedulingIgnoredDuringExecution: {}
        preferredDuringSchedulingIgnoredDuringExecution:
          weight: 1 # Between 1 - 100
      activeMetadata:
        labels:
          role: active
      previewMetadata:
        labels:
          role: preview
# images




docker.io/nginx:alpine3.18-slim
docker.io/httpd:2.4.58-alpine
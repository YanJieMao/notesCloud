# MutatingAdmissionWebhook如何工作

<img src="img/16dc4797adbda53f" alt="img" style="zoom:80%;" />

### Webhook Admission Server

`Webhook Admission Server` 只是一个附着到 k8s apiserver 的 http server。对于每一个 apiserver 的请求，`MutatingAdmissionWebhook` 都会发送一个 `admissionReview` 到相关的 `webhook admission server`。`webhook admission server` 再决定如何更改资源。


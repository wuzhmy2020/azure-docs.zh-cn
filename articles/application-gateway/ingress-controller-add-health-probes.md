---
title: 向 AKS Pod 添加运行状况探测
description: 本文介绍如何使用应用程序网关向 AKS Pod 添加运行状况探测（就绪情况和/或运行情况）。
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 5d0543a3a43d53e462a6406312faddf37d2653c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "73795600"
---
# <a name="add-health-probes-to-your-service"></a>向服务添加运行状况探测
默认情况下，入口控制器将为公开的 Pod 预配 HTTP GET 探测。
可以通过向`pod`规范添加[就绪或动态探测](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/)`deployment`/来自定义探测器属性。

## <a name="with-readinessprobe-or-livenessprobe"></a>使用 `readinessProbe` 或 `livenessProbe`
```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: aspnetapp
spec:
  replicas: 3
  template:
    metadata:
      labels:
        service: site
    spec:
      containers:
      - name: aspnetapp
        image: mcr.microsoft.com/dotnet/core/samples:aspnetapp
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 80
        readinessProbe:
          httpGet:
            path: /
            port: 80
          periodSeconds: 3
          timeoutSeconds: 1
```

Kubernetes API 参考：
* [容器探测](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes)
* [HttpGet 操作](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.14/#httpgetaction-v1-core)

> [!NOTE]
> * 配置了 `httpGet` 时，支持 `readinessProbe` 和 `livenessProbe`。
> * 当前不支持对 Pod 上公开的端口以外的端口进行探测。
> * 不支持 `HttpHeaders`、`InitialDelaySeconds`、`SuccessThreshold`。

##  <a name="without-readinessprobe-or-livenessprobe"></a>不使用 `readinessProbe` 或 `livenessProbe`
如果未提供上述探测，则入口控制器假设在为 `backend-path-prefix` 注释指定的 `Path` 上或在服务的 `ingress` 定义中指定的 `path` 上可以访问该服务。

## <a name="default-values-for-health-probe"></a>运行状况探测的默认值
对于就绪情况/运行情况探测无法推断的任何属性，将设置默认值。

| 应用程序网关探测属性 | 默认值 |
|-|-|
| `Path` | / |
| `Host` | localhost |
| `Protocol` | HTTP |
| `Timeout` | 30 |
| `Interval` | 30 |
| `UnhealthyThreshold` | 3 |
---
title: "Breaking Changes"
description: "Upcoming and recent breaking changes in ACK controllers"
lead: "Default behavior changes that may require user action"
draft: false
menu:
  docs:
    parent: "introduction"
    weight: 16
toc: true
---

## Cross-Namespace References Default Change

{{% hint type="warning" title="Breaking Change Notice" %}}
The default value of `--enable-cross-namespace` will change from `true` to `false` in a future release.
{{% /hint %}}

### Summary

Cross-namespace resource references (including `*Ref` fields, `SecretKeyReference`, and `FieldExport` targets across namespaces) will require explicit opt-in to improve namespace isolation.

### Timeline

| Phase | Description |
|-------|-------------|
| **Phase 1 (current)** | Flag added with default `true`. Warning condition set on resources using cross-namespace references |
| **Phase 2 (future release)** | Flag default changes to `false`. Cross-namespace references rejected unless opted in |

### Who is affected

You are affected if any of your ACK resources:
- Use `*Ref` fields pointing to resources in a **different namespace**
- Use `SecretKeyReference` with a `namespace` field different from the resource's namespace
- Use `FieldExport` CRs targeting ConfigMaps/Secrets in a different namespace than the source

If all your references are within the same namespace, **no action is needed**.

### Action required

If you use cross-namespace references, explicitly opt in before upgrading to the Phase 2 release:

```yaml
# values.yaml
enableCrossNamespace: true
```

Or via controller flag:
```
--enable-cross-namespace=true
```

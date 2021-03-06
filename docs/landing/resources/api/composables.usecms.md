<!-- Do not edit this file. It is automatically generated by API Documenter. -->

[Home](./index.md) &gt; [@shopware-pwa/composables](./composables.md) &gt; [useCms](./composables.usecms.md)

## useCms() function

> This API is provided as a preview for developers and may change based on feedback that we receive. Do not use this API in a production environment.
> 


<b>Signature:</b>

```typescript
export declare function useCms(rootContext: ApplicationVueContext): {
    page: Ref<Readonly<PageResolverProductResult | PageResolverResult<CmsPage>>>;
    categoryId: ComputedRef<string>;
    loading: Ref<boolean>;
    search: (path: string, query?: any) => Promise<void>;
    error: Ref<any>;
    getBreadcrumbsObject: ComputedRef<PageBreadcrumb>;
};
```

## Parameters

|  Parameter | Type | Description |
|  --- | --- | --- |
|  rootContext | [ApplicationVueContext](./composables.applicationvuecontext.md) |  |

<b>Returns:</b>

{ page: Ref&lt;Readonly&lt;PageResolverProductResult \| PageResolverResult&lt;CmsPage&gt;&gt;&gt;; categoryId: ComputedRef&lt;string&gt;; loading: Ref&lt;boolean&gt;; search: (path: string, query?: any) =&gt; Promise&lt;void&gt;; error: Ref&lt;any&gt;; getBreadcrumbsObject: ComputedRef&lt;PageBreadcrumb&gt;; }


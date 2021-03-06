<template>
  <div :key="$route.path">
    <component :is="getComponent" :cms-page="cmsPage" :page="page" />
  </div>
</template>
<script>
import { useCms, useNotifications } from "@shopware-pwa/composables"

const pagesMap = {
  "frontend.navigation.page": () =>
    import("@/components/views/CategoryView.vue"),
  "frontend.detail.page": () => import("@/components/views/ProductView.vue"),
}

export function getComponentBy(resourceType) {
  if (!resourceType || !pagesMap[resourceType]) return
  return pagesMap[resourceType]
}

export default {
  name: "DynamicRoute",
  components: {},
  watchQuery(newQuery, oldQuery) {
    // Only execute component methods if currency changed
    return newQuery.currencyId !== oldQuery.currencyId
  },
  asyncData: async ({ params, app, error: errorView, query, redirect }) => {
    const { search, page, error } = useCms(app)
    const { pushError } = useNotifications(app)
    const searchResult = await search(params.pathMatch, query)

    // direct user to the error page (keep http status code - so do not redirect)
    if (error.value) {
      if (error.value.statusCode === 404) {
        errorView(error.value)
      } else {
        // show the error other than 404 in SwNotifications
        pushError(error.value.message)
      }
    }

    const unwrappedPage = page && page.value ? page.value : searchResult
    const name =
      unwrappedPage && unwrappedPage.cmsPage && unwrappedPage.cmsPage.name
    const cmsPage = unwrappedPage && unwrappedPage.cmsPage

    // redirect to the cannnical URL if current path does not match the canonical one
    if (params.pathMatch && unwrappedPage && unwrappedPage.canonicalPathInfo) {
      if (unwrappedPage.canonicalPathInfo !== params.pathMatch) {
        redirect(301, app.$routing.getUrl(unwrappedPage.canonicalPathInfo))
      }
    }

    return {
      cmsPageName: name,
      page: unwrappedPage,
      cmsPage,
    }
  },
  computed: {
    getComponent() {
      return this.page && getComponentBy(this.page.resourceType)
    },
  },
  methods: {},
}
</script>
<style lang="scss" scoped>
@import "@/assets/scss/variables";
</style>

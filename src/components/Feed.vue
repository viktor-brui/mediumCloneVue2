<template>
    <div>
        <div v-if="isLoggedIn">Loading...</div>
        <div v-if="error">Error...</div>
        <div v-if="feed">
            <div class="article-preview" v-for="(article, index) in feed.articles" :key="index">
                <div class="article-meta">
                    <router-link :to="{name: 'userProfile', params: {slug: article.author.username}}">
                        <img :src="article.author.image" alt="user_icon">
                    </router-link>
                    <div class="info">
                        <router-link :to="{name: 'userProfile', params: {slug: article.author.username}}" class="author">
                            {{article.author.username}}
                        </router-link>
                        <span class="date">{{article.createdAt}}</span>
                    </div>
                    <div class="pull-xs-right">
                        Add to favorites
                    </div>
                </div>
                <router-link :to="{name: 'article', params: {slug: article.slug}}" class="preview-link">
                    <hi>{{article.title}}</hi>
                    <p>{{article.description}}</p>
                    <span>Read more...</span>
                    TAG LIST
                </router-link>
            </div>
            PAGINATION
<!--            <mcv-pagination :total="feed.articlesCount" :limit="limit" :current-page="currentPage" :url="baseUrl"/>-->
            <mcv-pagination :total="feed.articlesCount" :limit="limit" :current-page="currentPage" :url="baseUrl"/>
        </div>
    </div>
</template>

<script>
    import {mapState} from 'vuex'
    import {actionType} from "@/store/modules/feed";
    import McvPagination from "@/components/Pagination"
    import {limit} from "@/helpers/vars";
    import {stringify, parseUrl} from 'query-string'

    export default {
        name: "McvFeed",
        props: {
            apiUrl: {
                type: String,
                required: true,
            }
        },
        components: {
          McvPagination,
        },
        data() {
            return {
                limit,
                url: '/'
                // limit,
                // url: '/'
            }
        },
        computed: {
            ...mapState({
                isLoggedIn: state => state.feed.isLoading,
                feed: state => state.feed.data,
                error: state => state.feed.error,
            }),
            currentPage() {
                return Number(this.$router.query.page || '1')
            },
            baseUrl() {
              return this.$route.path
            },
            offset() {
                return this.currentPage * limit - limit
            }
        },
        watch: {
            currentPage() {
                this.fetchFeed()
            }
        },
        mounted() {
            console.log('init feed')
            this.$store.dispatch(actionType.getFeed, {apiUrl: this.apiUrl})
            // this.fetchFeed()
        },
        methods: {
            fetchFeed() {
                const parsedUrl = parseUrl(this.apiUrl)
                const stringifiedParams = stringify({
                    limit,
                    offset: this.offset,
                    ...parsedUrl.query,
                })
                const apiUrlWithParams = `${parsedUrl.url}?${stringifiedParams}`
                this.$store.dispatch(actionType.getFeed, {apiUrl: apiUrlWithParams})
            }
        }
    }
</script>

<style scoped>

</style>
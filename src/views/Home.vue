<template>
    <my-page title="快递查询">
        <div class="common-container container">
            <ui-text-field v-model="no" label="快递单号" />
            <br>
            <ui-select-field v-model="company" label="快递公司">
                <ui-menu-item v-for="item,index in companies" 
                    :key="index"
                    :value="item.no"
                    :title="item.com" />
            </ui-select-field>
            <div class="btns">
                <ui-raised-button class="btn" label="查询" primary @click="query" />
            </div>
            <ui-timeline v-if="result">
                <ui-timeline-item v-for="item in result.list" :key="item.datetime">
                    <span slot="time">{{ item.datetime }}</span>
                    <span slot="des">{{ item.remark }}</span>
                </ui-timeline-item>
            </ui-timeline>
        </div>
    </my-page>
</template>

<script>
    export default {
        data () {
            return {
                companies: [],
                company: '',
                no: '',
                result: null,
                page: {
                    menu: [
                        {
                            type: 'icon',
                            icon: 'help',
                            to: '/help'
                        }
                    ]
                }
            }
        },
        mounted() {
            this.init()
            // this.debug()
        },
        methods: {
            init() {
                this.$http.get('/express/companies').then(
                response => {
                    let data = response.data
                    console.log(data)
                    this.companies = data
                    // this.company = 'ht'
                },
                response => {
                    console.log(response)
                })
            },
            debug() {
                this.no = '71259693063077'
                this.result = {
                    list: [
                        {
                            datetime: '2017-12-27 14:27:15',
                            remark: '绍兴市【诸暨大唐二部】，【百世快递xxx】已揽收'
                        },
                        {
                            datetime: '2017-12-27 14:27:15',
                            remark: '绍兴市【诸暨大唐二部】，【百世快递xxx】已揽收'
                        }
                    ]
                }
            },
            query() {
                if (!this.no) {
                    this.$message({
                        type: 'danger',
                        text: '请输入快递单号'
                    })
                    return
                }
                if (!this.company) {
                    this.$message({
                        type: 'danger',
                        text: '请选择快递公司'
                    })
                    return
                }
                this.$http.get(`/express?company=${this.company}&no=${this.no}`).then(
                response => {
                    let data = response.data
                    console.log(data)
                    this.result = data.result
                },
                response => {
                    console.log(response)
                })
            }
        }
    }
</script>

<style scoped>
.btns {
    margin-bottom: 16px;
}
</style>

/**
* @Date: 2016/10/22  0022  12:07
* @Author: lxbin
* Created with JetBrains WebStorm.
*/
<template>
    <div class="mianlayer1">
        <div class="box">
            <div class="navlist1">
                <div class="left">
                    <ul>
                        <li :class="{hover:leftQuery.quyu}" @click="chageLeft('quyu')">区域</li>
                        <li :class="{hover:leftQuery.ditie}" @click="chageLeft('ditie')">地铁</li>
                    </ul>
                </div>
                <div class="right">
                    <ol v-show="leftQuery.quyu">
                        <li @click="selectQuyu('','不限')"
                            :class="{hover:indexSearch.quyu==''}">
                            不限
                        </li>
                        <li v-for="item in baseInfo.quYuList"
                            :id="item.id"
                            @click="selectQuyu(item.id,item.t_name)"
                            :class="{hover:indexSearch.quyu==item.id}">
                            {{item.t_name}}
                        </li>
                    </ol>
                    <ol v-show="leftQuery.ditie">
                        <li @click="selectDitie('','不限')"
                            :class="{hover:indexSearch.ditie==''}">
                            不限
                        </li>
                        <li v-for="item in baseInfo.diTieList"
                            :id="item.id"
                            @click="selectDitie(item.id,item.t_name)"
                            :class="{hover:indexSearch.ditie==item.id}">
                            {{item.t_name}}
                        </li>
                    </ol>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    import {mapState, mapGetters, mapActions} from 'vuex'
    import * as types from '../store/mutation-types'
    export default{
        name: 'search-quyu',
        props: [],
        data(){
            return {
                leftQuery: {
                    quyu: true,
                    ditie: false
                },
                selected: {
                    quyu: '',
                    ditie: ''
                }
            }
        },
        computed: {
            ...mapState({
                indexSearch: state=>state.base.indexSearch
            }),
            ...mapGetters({
                baseInfo: 'baseInfo'
            })
        },
        methods: {
            ...mapActions([
                'setRoadList',
                'setMapList'
            ]),
            chageLeft(type){
                for (let query in this.leftQuery)
                    this.leftQuery[query] = false
                this.leftQuery[type] = true
            },
            selectQuyu(type, title){
                this.selected.quyu = type
                this.$store.commit(types.SET_INDEX_SEARCH_INFO, this.selected)
                this.$store.state.base.tempVm.$emit('closeSerchInfo')
                this.$store.state.base.tempVm.$emit('setSelectedName', {quyu: title})
            },
            selectDitie(type, title){
                this.selected.ditie = type
                this.$store.commit(types.SET_INDEX_SEARCH_INFO, this.selected)
                this.$store.state.base.tempVm.$emit('closeSerchInfo')
                this.$store.state.base.tempVm.$emit('setSelectedName', {ditie: title})
            }
        },
        created(){
            const _vm = this
            console.log('[Leo] => ',_vm.indexSearch.quyu)
            _vm.selected.quyu = _vm.indexSearch.quyu
            _vm.selected.ditie = _vm.indexSearch.ditie
        }
    }
</script>
<style scoped>
    .navlist2 li.hover {
        background: #e2e2e2;
    }
</style>

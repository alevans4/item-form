<template>
    <item-form :item="item"></item-form>
</template>
<script type="text/babel">
    // import {getCookieId} from '../../Store/actions'
    export default {
        data: function () {
            return {
                item: {
                    type: ''
                }
            }
        },
        created: function () {
            // this.$http.get('/item/' + this.$route.params.id)
            //     .then(function (response) {
            //         this.item = response.data;
            //         if(this.item.is_auction){
            //             this.item.type = 'auction';
            //         }else if(this.item.direct_sale_price){
            //             this.item.type = 'direct-sale';
            //         }else if(this.item.is_tender){
            //             this.item.type = 'tender';
            //         }else{
            //             this.item.type = 'plain';
            //         }
            //     });
        },
        components: {
            'item-form': require('./ItemForm.vue')
        }
    }
</script>
<template>
    <div>
        <div class="row">
            <div class="large-12 columns">
                <div class="row">
                     <div class="small-2 columns">
                        <label class="left inline">Type</label>
                    </div>
                    <div class="small-6 columns float-left">
                        <select v-model="item.type">
                            <option value="plain">Plain</option>
                            <option value="auction">Auction</option>
                            <option value="direct-sale">Direct sale</option>
                            <option value="tender">Tender</option>
                        </select>
 
                        <div class="row" v-if="item.type == 'direct-sale' || item.type == 'auction'">
                            <div class="small-4 columns">
                                <label class="left inline">Price</label>
                            </div>
                            <div class="small-6 columns float-left">
                                <input class="large-2 float-left" type="text" v-model="item.direct_sale_price" placeholder="value">
                            </div>
                        </div>
                        <div class="row" v-if="item.type == 'auction'">
                            <div class="small-4 columns">
                                <label for="date" class="left inline">End date</label>
                            </div>
                            <div class="small-6 columns float-left">
                                <datepicker id="date" :disabled="disabled" v-model="item.auction_end_date"></datepicker>
                            </div>
                        </div>
                        <div class="row" v-if="item.type == 'tender'">
                            <div class="row columns">
                                <div class="small-4 columns">
                                    <label class="left inline">Maximum Price</label>
                                </div>
                                <div class="small-6 columns float-left">
                                    <input class="large-2 float-left" type="text" v-model="item.tender_max_price" placeholder="Price">
                                </div>
                            </div>
                            <div class="row columns">
                                <div class="small-4 columns">
                                    <label for="tenderEndDate" class="left inline">End date</label>
                                </div>
                                <div class="small-6 columns float-left">
                                    <datepicker id="tenderEndDate" :disabled="disabled" v-model="item.tender_end_date"></datepicker>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="small-2 columns">
                        <label for="image" class="left inline">Image</label>
                    </div>
                    <div class="small-3 columns float-left">
                        <button class="file-upload button">
                            <input type="file" class="file-input" id="image" @change="sync">
                                Choose File
                        </button>
                    </div>
                </div>
            </div>
            <div class="large-8 columns float-left">
                <button v-on:keydown.enter.prevent='submitItem' type="button" @click="submitItem" class="button primary float-right">Save Item</button>
            </div>
        </div>
    </div>
</template>
<script type="text/babel">
import Datepicker from 'vuejs-datepicker'; // https://github.com/charliekassel/vuejs-datepicker
export default {
    name: 'item-form',
    props: ['item'],
    data() {
        return {
            direct_sale_price: '',
            auction_end_date: '',
            disabled: {
                to: new Date(new Date().setDate(new Date().getDate()-1))
            },
        }
    },
    methods: {
        sync: function (e) {
            e.preventDefault();
            this.image = e.target.files[0]
        },
        submitItem: function () {
            let self = this;
            let formData = new FormData();
            formData.append('image_file', self.image);
            formData.append('item_type', self.item.type);
            if(self.item.type === 'direct-sale' || self.item.type === 'auction'){
                formData.append('direct_sale_price', self.item.direct_sale_price)
            }
            if(self.item.type === 'auction'){
                formData.append('auctionEndDate', self.item.auction_end_date)
            }
            if(self.item.type === 'tender'){
                formData.append('tender_end_date', self.item.tender_end_date);
                formData.append('tender_max_price', self.item.tender_max_price)
            }
 
            let post_url = '';
            if (self.item.item_id) {
                post_url = self.$http.put('/user/' + self.$root.$store.state.cookieUserId + '/items/' + self.item.item_id, formData)
            }
            else {
                post_url = self.$http.post('/user/' + self.$root.$store.state.cookieUserId + '/items', formData)
            }
            post_url.then(response => {
                self.$router.push('/item/' + response.data.item_id + '/details');
            }, response => {
                console.log("error");
                console.log(response)
            })
 
        }
    },
    components: {
        Datepicker
    }
}
</script>
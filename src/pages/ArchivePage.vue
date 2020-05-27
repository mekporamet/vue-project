<template>
    <div>
        <card-list :items="archives"></card-list>
    </div>
</template>>

<script>
import CardList from '../components/CardList.vue'
import axios from 'axios'
export default {
    name:'ArchivePage',
    components:{
        CardList
    },
    async created(){
        const {data}= await axios.get('http://localhost:3000/archives')
        this.archives = data
    },
    data(){
        return{
            archives:[]
        }
    }
    
}
</script>>
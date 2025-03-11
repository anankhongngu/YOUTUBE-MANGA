<template>
    <div class="w3-row-padding d-flex">
        <div class="banner">
            <h1 class="banner-title">{{ manga.title }}</h1>
        </div>
        <div class="w3-col-25 w3-container  sticky">
            <div class="w3-white w3-margin-bottom">
                <div class="player wapper-player">
                    <div class="title">
                        <h4>Reading:</h4>
                        <h2>{{ manga.title }}</h2>
                        <p>{{ manga.description }}</p>
                    </div>
                    <div class="cd">
                        <!-- <router-link></router-link> -->
                            <div class="cd-thumb" ref="cdThumb" :style="{ backgroundImage: `url(${ manga.cover })` }"></div>
                        
                    </div> 
                    <div class="chapter">
                        <h2>Các chapter:</h2>
                        <ul>
                            <li v-for="chapter in chapters" :key="chapter.id">
                                <a class="anbcas" @click="fetchChaptersDetail(chapter.id)"
                                :class="{'active':  currentID === chapter.id}" 
                                >{{ chapter.chap }}. {{ chapter.title }}</a>
                            </li>  
                        </ul>
                    </div>
                    <div class="control">
                        <div class="btn btn-prev">
                            <i class="fa fa-step-backward"></i>
                        </div>
                        <div class="btn btn-next">
                            <i class="fa fa-step-forward"></i>
                        </div>
                    </div>
                </div>
            </div>  
        </div>
        <div class="w3-col-75 w3-container">
            <div class="manga-section">
                <div class="manga-detail" v-if="listImgChap.length">
                    <div class="manga-item w3-margin-bottom" v-for="(imgChap,index) in listImgChap"> 
                        <img :src="imgChap.imgChapter" :alt="manga.title">
                    </div>
                </div>
            </div>
        </div>
        
    </div>
</template>
<script>
import axios from 'axios';
export default {
    props: ['id'],
    data() {
        return {
            isActive: false,
            currentID: null,
            manga: {
                title: '',
                cover: '',
                description: ''
            },
            chapters: [],
            listImgChap: []
        }
    },
    async mounted() {
        await this.fetchMangaDetails()
        await this.fetchListChapters()
    },
    computed: {
       
    },
    methods: {
        async fetchMangaDetails () {
            const response = await axios.get(`https://api.mangadex.org/manga/${this.id}?includes[]=cover_art`)
            const data = response.data.data

            const cover = data.relationships.find(item => item.type === 'cover_art')
            this.manga = {
                title: this.getTitle(data.attributes),
                description: this.getDescription(data.attributes),
                cover: cover ? `https://uploads.mangadex.org/covers/${this.id}/${cover.attributes.fileName}` : ''
            }
        },
        async fetchListChapters () {
            const response = await axios.get(`https://api.mangadex.org/manga/${this.id}/feed?translatedLanguage[]=vi&order[chapter]=asc`)
            this.chapters = response.data.data.map(chap => ({
                chap: chap.attributes.chapter,
                id: chap.id,
                title: chap.attributes.title || `Chapter ${chap.attributes.chapter}`,
            }))
            //console.log(this.chapters[0])
            this.fetchChaptersDetail(this.chapters[0].id)
        },      
        getTitle (attributes) {
            return attributes.title.vi || attributes.title.en || attributes.title.ja || 'Chưa có tiêu đề';
        },
        getDescription (attributes) {
            return attributes.description.vi || attributes.description.en || attributes.description.ja || 'Đang cập nhật mô tả';
        },
        async fetchChaptersDetail (id){
            this.listImgChap = [];
            window.scrollTo({ top: 0, behavior: 'smooth' })
            this.currentID = id
            const response = await axios.get(`https://api.mangadex.org/at-home/server/${id}`);
            this.listImgChap = response.data.chapter.data.map(img => ({
                imgChapter: img ? `https://uploads.mangadex.org/data/${response.data.chapter.hash}/${img}` : ''
            }))
        },
    }
}
</script>
<style>
    .chapter li a.active {
        color: var(--icon-green);
        text-decoration: underline;
        font-weight: bold;
    }
</style>
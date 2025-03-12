<template>
    <nav class="w3-sidebar w3-collapse w3-animate-left" style="z-index:3;width:300px;" id="mySidebar">
        <br>

        <div class="w3-container">
            <a href="#" @click="sidebarClose()" class="icon-color w3-hide-large w3-right w3-jumbo w3-padding w3-hover-grey" title="close menu">
                <i class="fa fa-remove"></i>
            </a>
            <a href="/"><img src="./../../assets/image/avatar/avatar_1.PNG" style="width: 50%;" class="w3-round" alt="My protfolio"></a>
            <br><br>
            <h4 class="title--n"><b>LÊ TRƯỜNG AN</b></h4>
            <p class="text-slogan hidden">khongbaogionhanminhngu</p>
        </div>

        <div class="w3-bar-block">
            <router-link :to="`/resume`" :class="{ active: $route.path === '/resume' }" class="text-nav w3-bar-item w3-button w3-padding">    
                <i class="fa fa-address-card-o fa-fw w3-margin-right"></i>RESUME
            </router-link>

            <a href="mailto:truongan2700@gmail.com" class="text-nav w3-bar-item w3-button w3-padding">
                <i class="fa fa-envelope w3-margin-right"></i>CONTACT
            </a>
            <router-link :to="`/manga/search`" v-if="isActive('/manga')" :class="{ active: isActive('/manga') }" class="text-nav w3-bar-item w3-button w3-padding">    
                <i class="fa fa-book fa-fw w3-margin-right"></i>Manga
            </router-link>
        </div>  

        <div class="w3-panel w3-large">
            <a href="https://fb.com/truongan15" class="w3-margin-right icon-color">
                <i class="fa fa-facebook-official w3-hover-opacity"></i>
            </a>  
        </div>
        <div class="theme-switch-wrapper">
            <label for="checkbox" class="theme-switch">
                <input @change="switchTheme" :class="current" type="checkbox" id="checkbox">
                <div class="slider-cb round"></div>
            </label>
        </div> 
    </nav> 
    <div class="w3-overlay w3-hide-large w3-animate-opacity" @click="sidebarClose()" style="cursor:pointer; display: none;" title="close side menu" id="myOverlay"></div>
</template>

<script>
    export default {
        data() {
            return {
                current: localStorage.getItem('theme')
            }
        },       
        setup() {
            //const toggleSwitch = document.querySelector('.theme-switch input[type="checkbox"]');
            const currentTheme = localStorage.getItem('theme');
            if (currentTheme) {
                document.documentElement.setAttribute('data-theme', currentTheme);
            }   
        },
        methods: {           
            switchTheme(e) {
                if (e.target.classList.contains('dark')) {
                    document.documentElement.setAttribute('data-theme', 'light');
                    localStorage.setItem('theme', 'light');
                    this.current = 'light';
                } else {
                    document.documentElement.setAttribute('data-theme', 'dark');
                    localStorage.setItem('theme', 'dark');
                    this.current = 'dark';
                } 
            },
            sidebarOpen() {
                document.getElementById("mySidebar").style.display = "block";
                document.getElementById("myOverlay").style.display = "block";
            },
            sidebarClose() {
                document.getElementById("mySidebar").style.display = "none";
                document.getElementById("myOverlay").style.display = "none";
            },
            isActive(route) {
                //console.log(this.$route.path.startsWith(route))
                return this.$route.path.startsWith(route);
            },
        }            
    }
</script>

<style>
    .text-nav {
        color: var(--text-black);
    }
    .active.text-nav {
        color: var(--icon-green);
        font-weight: bold;
    }
</style>
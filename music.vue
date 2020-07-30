var app = new Vue({
    el: "#player",
    data:{
        query:"",
        musicList: [],
        musicUrl:"",
        musicCover: "picture/music_disk.jpg",
        hotComments:[],
        isShow: false,
        mvUrl:"",
        //isPlaying:false,
    },
    methods:{
        searchMusic:function(){
            var that = this;
            // Music name list
            axios.get("https://autumnfish.cn/search?keywords="+this.query)
                .then(function(response){
                    //console.log(response);
                    that.musicList = response.data.result.songs;
                },function(err){})
        },

        playMusic:function(musicId){
            var that = this;
            // Music playing
            axios.get("https://autumnfish.cn/song/url?id="+musicId)
                .then(function(response){
                    //console.log(musicId);
                    //console.log(response);
                    //console.log(response.data.data[0].url);
                    that.musicUrl=response.data.data[0].url;
            },function(err){});

            // Music cover
            axios.get("https://autumnfish.cn/song/detail?ids="+musicId)
                .then(function(response){
                    //console.log(response.data.songs[0].al.picUrl);
                    that.musicCover = response.data.songs[0].al.picUrl;
                }, function(err){});

            // Comment
            axios.get("https://autumnfish.cn/comment/hot?type=0&id="+musicId)
                .then(function(response){
                    //console.log(response);
                    //console.log(response.data.hotComments);
                    that.hotComments= response.data.hotComments
                },function(err){});
        },

        playMV:function(mvid){
            var that =this;
            axios.get("https://autumnfish.cn/mv/url?id="+ mvid)
                .then(function(response){
                    //console.log(response.data.data.url);
                    that.isShow=true;
                    that.mvUrl = response.data.data.url;
                },function(err){})
        },

        //pauseMV:function(){},

        hide:function(){
          this.isShow= false;
          this.mvUrl ="";
        },

        /*
        play:function(){
            this.isPlaying =true;
        },
        pause:function(){
            this.isPlaying = false;
        },
        */
    }
})
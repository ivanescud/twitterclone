<template>
  
  <div class="flex container h-screen w-full ">
  <!--side nav-->
    <div class="lg:w-1/5 border-r border-lighter lpx-2 lg:px-6 py-2 flex flex-col justify-between">
      <div >
        <button class="h-12 w-12 hover:bg-lightblue text-3xl text-blue rounded-full ml-3">
          <i class="fab fa-twitter"></i>
        </button>
      <div>
        <button  v-for="(item, index) in tabs" :key="index" @click="id = item.id" :class="`focus:outline-none hover:text-blue flex items-center py-2 px-4 hover:bg-lightblue rounded-full mr-auto mb-3 ${ id === item.id ? 'text-blue' : ''} `">
          <i :class="`${item.icon} text-2xl mr-4 text-left ml-2`"></i>
          <p class="text-lg font-semibold text-left hidden lg:block">{{item.title}}</p>
        </button>
      </div>
      <button class="text-white bg-blue rounded-full font-semibold focus:outline-none w-12 h-12 lg:h-auto px-2 lg:w-full ml-3 hover:bg-darkblue">
      <p class="hidden lg:block">Tweet</p>
       <i class="fas fa-plus lg:hidden"></i>
     
      </button>
      </div>
      <div class="w-full relative">
      <button @click="dropdown = true" class="flex items-center w-full hover:bg-lightblue rounded-full p-2 focus:outline-none">
      <img :src="profile" class="w-12 h-12 rounded-full border border-lighter"/>
      <div class="ml-4 lg:block hidden">
        <p class="text-sm font-bold leading-tight">Laura Lopez</p>
        <p class="text-sm ">@laurilla</p>
      </div>
      <i class="fas fa-angle-down ml-auto"></i>
      </button>
      <div v-if="dropdown === true" class="absolute bottom-0 left-0 w-64 rounded-lg shadow-md border-lightest bg-white mb-16">
        <button @click="dropdown = false" class=" p-3 flex items-center w-full hover:bg-lightest p-2">
          <img :src="profile" class="w-12 h-12 rounded-full border border-lighter"/>
          <div class="ml-4">
            <p class="text-sm font-bold leading-tight">Laura Lopez</p>
            <p class="text-sm ">@laurilla</p>
          </div>
          <i class="fas fa-check-down test-blue ml-auto"></i>
        </button>
        <button @click="dropdown = false" class=" w-full text-left hover:bg-lightest border-t border-lighter p-3 focus:outline-none">
        Add and existing account
        </button>
        <button @click="dropdown = false" class="w-full text-left hover:bg-lightest border-t border-lighter p-3 focus:outline-none">
        Log out @laurilla
        </button>
      </div>
    </div>
  </div>
    <!--- Tweets -->
    <div class="w-1/2 h-full overflow-y-scroll">
      <div class="px-5 py-3 border-b border-lighter flex items-center justify-between">
        <h1 class="text-xl font-bold">
        Home
        </h1>
        <i class="far fa-star text-xl text-blue"></i>
      </div>
      <div class="px-5 py-3 border-b-8 border-lighter flex">
        <div>
          <img :src="profile" class="w-12 h-12 rounded-full border-lighter">
        </div>
        <form v-on:submit.prevent="addNewTweet" class="w-full px-4 relative">
        <textarea v-model="tweet.content" placeholder="What's up?" class="w-full mt-3 pb-3  focus:outline-none"></textarea>
        <div class="flex items-center">
          <i class="text-lg text-blue mr-4  far fa-image"></i>
          <i class="text-lg text-blue mr-4 fas fa-film"></i>
          <i class="text-lg text-blue mr-4 far fa-chart-bar"></i>
          <i class="text-lg text-blue mr-4 far fa-smile"></i>
        
        </div>
        <button type="submit" class="h-10 px-4 text-white font-semibold bg-blue hover:bg-darkblue focus:outline-none rounded-full absolute bottom-0 right-0">
          Tweet
        </button>
        </form>
      </div>

       <div v-for="(tweet, index) in tweets" :key="index"  class="w-full p-4 border-b hover:bg-lighter flex">
        <div class="flex-none mr-4">
          <img :src="profile" class="h-12 w-12 rounded-full flex-none" />
        </div>
        <div class="w-full"> 
             <div class="flex items-center w-full"> 
              <p class="font-semibold">Laura Lopez</p>
              <p class="text-sm text-dark ml-2">@laurilla</p>
              <p class="text-sm text-dark ml-2">1 sec</p>
              <i class="fas fa-angle-down text-dark ml-auto"></i>
            </div>
            <p class="py-2">
            {{tweet.content}}
            </p>
            <div class="flex items-center justify-between w-full"> 
              <div class="flex items-center text-sm text-dark">
                <i class="mr-3 far fa-comment">
                </i>
                <p>0</p>
              </div>
               <div class="flex items-center text-sm text-dark">
                <i class="mr-3 fas fa-retweet">
                </i>
                <p>10</p>
              </div>
               <div class="flex items-center text-sm text-dark">
                <i class="mr-3 fas fa-heart ">
                </i>
                <p>20</p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="mr-3 fas fa-share-square">
                </i>
                
              </div>
               
            </div>
        </div>
      </div>

      <div v-for="(follow, index) in following" :key="index"  class="w-full p-4 border-b hover:bg-lighter flex">
        <div class="flex-none mr-4">
          <img :src="follow.src" class="h-12 w-12 rounded-full flex-none" />
        </div>
        <div class="w-full"> 
             <div class="flex items-center w-full"> 
              <p class="font-semibold">{{follow.name}}</p>
              <p class="text-sm text-dark ml-2">{{follow.handle}}</p>
              <p class="text-sm text-dark ml-2">{{follow.time}}</p>
              <i class="fas fa-angle-down text-dark ml-auto"></i>
            </div>
            <p class="py-2">
            {{follow.tweet}}
            </p>
            <div class="flex items-center justify-between w-full"> 
              <div class="flex items-center text-sm text-dark">
                <i class="mr-3 far fa-comment">
                </i>
                <p>{{follow.comments}}</p>
              </div>
               <div class="flex items-center text-sm text-dark">
                <i class="mr-3 fas fa-retweet">
                </i>
                <p>{{follow.retweets}}</p>
              </div>
               <div class="flex items-center text-sm text-dark">
                <i class="mr-3 fas fa-heart ">
                </i>
                <p>{{follow.like}}</p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="mr-3 fas fa-share-square">
                </i>
                
              </div>
               
            </div>
        </div>
      </div>

    </div>
    <!-- Trending -->
    <div class="md:block hidden w-1/3 h-full border-l border-lighter py-2 px-6 overflow-y-scroll relative">
      <input class=" pl-12 rounded-full w-full p-2 bg-lighter text-sm mb-4" placeholder="Search Twitter"/>
      <i class="fas fa-search absolute left-0 top-0 mt-5 ml-12 text-sm text-light"></i>

      <div class="w-full rounded-lg bg-lightest">
        <div class="flex items-center justify-between p-3">
          <p class="text-lg font-bold">Trends for You</p>
          <i class="fas fa-cog text-lg text-blue"></i>
        </div>
        <button v-for="(trend, index) in trending" :key="index" class="w-full flex justify-between hover:bg-lighter p-3 border-t border-lighter">
            <div>
              <p class="text-sm text-left leading-tight text-dark">{{trend.top}}</p>
              <p class="font-bold text-left leading-tight">{{trend.title}}</p>
              <p class=" text-left leading-tight text-dark">{{trend.bottom}}</p>
            </div>
            <i class="fas fa-angle-down text-lg text-dark"></i>
      </button>
      <button class="p-3 w-full hover:bg-lighter text-left text-blue border-t border-lighter">
      Show More
      </button>
      </div>
        <div class="w-full rounded-lg bg-lightest my-4">
        <div class="p-3">
          <p class="text-lg font-bold">Who to Follow</p>
          
        </div>
        <button v-for="(friend, index) in friends" :key="index" class="w-full flex  hover:bg-lighter p-3 border-t border-lighter">
          <img :src="friend.src" class="w-12 h-12 rounded-full border border-lighter"/>
          <div class="ml-4 lg:block hidden">
            <p class="text-sm font-bold leading-tight">{{friend.name}}</p>
            <p class="text-sm ">{{friend.handle}}</p>
      </div>
      <button class=" ml-auto text-sm text-blue py-2 px-4 rounded-full border-2 border-blue">
      Follow 
      </button>
      </button>
      <button class="p-3 w-full hover:bg-lighter text-left text-blue border-t border-lighter">
      Show More
      </button>
      </div>


    </div>
</div>
  
</template>

<script>
export default {
  data(){
    return{
      tabs:[
        {icon:'fas fa-home', title:'Home', id:'home'},
        {icon:'fas fa-hashtag', title:'Explore', id:'explore'},
        {icon:'fas fa-bell', title:'Notifications', id:'notification'},
        {icon:'fas fa-envelope', title:'Messages', id:'messages'},
        {icon:'fas fa-bookmark', title:'Bookmarks', id:'bookmarks'},
        {icon:'fas fa-clipboard-list', title:'Lists', id:'lists'},
        {icon:'fas fa-user', title:'Profile', id:'profile'},
        {icon:'fas fa-ellipsis-h', title:'More', id:'more'}
      ],
      id:'home',
      dropdown: false,
      trending:[
        {top: 'Trending in TX', title:'Gigi', bottom:'Trending with : Rip Gigi'},
        {top: 'Music', title:'We Won', bottom:'135K Tweets'},
        {top: 'Pop', title:'Blue Ivy', bottom:'40 tweets'},
        {top: 'Trending in US', title:'Denim Day', bottom:'40k tweets'},
        {top: 'Trending', title:'When Beyonce', bottom:'25.4k tweets'},
      ],
      friends:[
        {src:'https://pbs.twimg.com/profile_images/1295975423654977537/dHw9JcrK_400x400.jpg' , name:'Elon Musk', handle:'@teslaBoy'},
        {src:'https://pbs.twimg.com/profile_images/288211039/Picture_2_400x400.png', name:'Adrian Monk', handle:'@dective'},
        {src:'https://pbs.twimg.com/profile_images/839198522385793025/jPENEMJT_400x400.jpg', name:'Kevin Hart', handle:'@miniRock'}
      ],
      profile:'https://images.unsplash.com/photo-1597223557154-721c1cecc4b0?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=800&q=80',
      following:[
        {src:'https://pbs.twimg.com/profile_images/1295975423654977537/dHw9JcrK_400x400.jpg', name:'Elon Musk' , handle:'@teslaBoy', time:'20 min', tweet:'Should I just quarantine on mars??', comments:'1,000', retweets:'200',like:'1,000,254'},
        {src:'https://pbs.twimg.com/profile_images/839198522385793025/jPENEMJT_400x400.jpg', name:'Kevin Hard' , handle:'@miniRock', time:'40 min', tweet:'Should me and rock train together', comments:'1,000', retweets:'400',like:'1,000,254'},
        {src:'https://pbs.twimg.com/profile_images/1295975423654977537/dHw9JcrK_400x400.jpg', name:'Elon Musk' , handle:'@teslaBoy', time:'55 min', tweet:'Lets go to venus now', comments:'1,000', retweets:'500',like:'1,000,254'},
        {src:'https://pbs.twimg.com/profile_images/1295975423654977537/dHw9JcrK_400x400.jpg', name:'Elon Musk' , handle:'@teslaBoy', time:'55 min', tweet:'Buen dia desde Panamá', comments:'1,000', retweets:'500',like:'1,000,254'}

      ],
      tweets:[
        {content:'It is so nice in Panamá'}
      ],
      tweet:{
        content:''
      }
    }
   
  },
   methods: {
      addNewTweet(){
        let newTweet = {
          content:this.tweet.content
        };
        this.tweets.push(newTweet)
      }
    }
}
</script>


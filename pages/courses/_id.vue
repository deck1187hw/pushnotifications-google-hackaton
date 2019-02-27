<template>
  <section class="container-fluid">
    <div>
     <div class="row">
       <div class="col text-center bg1 course-hero__background bg4">
         <h1>{{course.title}}</h1>
         <p class="lead">{{course.intro}}</p>
       </div>
     </div>

     <div class="row">
       <div class="col bg2">
        <div class="row">
            <div class="col text-center">
                <h2>Course overview</h2>
            </div>
        </div>
           <div class="row">
               <div class="col">
                   <h4 class="ttd">Description</h4>
                   <div class="desc1">{{course.description}}</div>
                   </div>
               <div class="col col-sm-5">
                   <h4 class="ttd">Lessons</h4>


<div class="course-modules clearfix">
<ul>
    <li v-for="(item, index) in course.lessons.lessons" class="course-module "> 
        <a href="#" title="What is financial trading?" class="clearfix">
    <h3>{{item.title}}</h3>
    <span class="duration">{{item.time}}</span> </a> 

    </li>
</ul>
</div>

               </div>
           </div>
        
         
        </div>
       </div>
     </div>

  </section>
</template>

<script>
import Logo from '~/components/Logo.vue'
import axios from 'axios'

export default {
  components: {
    Logo
  },
  data: function () {
    return {
      course: {}
    }
  },
  asyncData ({ params, error }) {
    return axios.get(`https://api.londongdhandball.co.uk/Courses/${params.id}`)
    .then((res) => {
      return { course: res.data }
    })
    .catch((e) => {
      error({ statusCode: 404, message: 'Post not found' })
    })
  }
}
</script>

<style>
.card-title{
  font-weight: 200;
  font-size: 1.2em;
}
.desc1{
        font-weight: 200;
        letter-spacing: 0.1px;
}
.ttd{
    text-transform: uppercase;
    color: #999;
    font-size: 1.2em;
    font-weight: 200;
    letter-spacing: 1px;
}
.card-text{
  font-size: 0.8em;
  color: #9a9a9a
}
.lead{
  font-weight: 200;
}
.bg1{
  background: #f5f6f8;
  padding: 40px 80px;
}
h2{
  margin-bottom: 50px;
}
.bg2{
  background: white;
  padding: 40px 80px;
}
.bg4{
    background: #353850;
    color: white;
}
h1{
    font-weight: bold
}

</style>

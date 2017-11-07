<template>
  <div>
    <div class="layout">
      <Row type="flex">
        <Col span="5" class="layout-menu-left">
        <Menu active-name="1-2" theme="dark" width="auto" :open-names="['1']">
          <div class="layout-logo-left"></div>
          <Submenu v-for="(school,index) in schools" :key="index" name="1" v-on:click.native="get_course(1)">
            <template slot="title" >
              <Icon type="ios-navigate"></Icon>
              {{school.name}}
            </template>
            <MenuItem name="1-1" v-for="course in courses" :key="course.id">{{course.name}}</MenuItem>
          </Submenu>
        </Menu>
        </Col>
        <Col span="19">
        <div class="layout-header"></div>
        <div class="layout-breadcrumb">
          <Breadcrumb>
            <BreadcrumbItem href="#">首页</BreadcrumbItem>
            <BreadcrumbItem href="#">应用中心</BreadcrumbItem>
            <BreadcrumbItem>某应用</BreadcrumbItem>
          </Breadcrumb>
        </div>
        <div class="layout-content">
          <div class="layout-content-main">内容区域</div>
        </div>
        <div class="layout-copy">
          2011-2016 &copy; TalkingData
        </div>
        </Col>
      </Row>
    </div>
  </div>
</template>

<script>
export default {
  name: 'splash',
  data () {
    return {
      courses:[],
      schools:[]
    }
  },
  mounted: function(){
    this.$nextTick(function () {
        this.get_school_list();
      });
  },
  methods:{
    get_school_list:function () {
      this.$http.post('https://finalexam.cn/api/school/list').then(response =>{
        this.schools = response.body.res;
      }, response => {
        // error callback
      });
    },
    get_course: function(school_num){
       this.$http.post('https://finalexam.cn/api/school/listAcademy',{college: school_num}).then(response =>{
       this.courses = response.body.res;
       }, response => {
       // error callback
     });
    },
  }
}
</script>


<style scoped>
  .layout{
    border: 1px solid #d7dde4;
    background: #f5f7f9;
    position: relative;
  }
  .layout-breadcrumb{
    padding: 10px 15px 0;
  }
  .layout-content{
    min-height: 1200px;
    margin: 15px;
    overflow: hidden;
    background: #fff;
    border-radius: 4px;
  }
  .layout-content-main{
    padding: 10px;
  }
  .layout-copy{
    text-align: center;
    padding: 10px 0 20px;
    color: #9ea7b4;
  }
  .layout-menu-left{
    background: #464c5b;
  }
  .layout-header{
    height: 60px;
    background: #fff;
    box-shadow: 0 1px 1px rgba(0,0,0,.1);
  }
  .layout-logo-left{
    width: 90%;
    height: 30px;
    background: #5b6270;
    border-radius: 3px;
    margin: 15px auto;
  }
</style>

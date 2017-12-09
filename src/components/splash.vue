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
        <div class="layout-content">
          <div class="layout-content-main">
            <iframe id="iframe_display" name="iframe_display" style="display: none;"></iframe>  
            <!-- <iframe :src="'https://view.officeapps.live.com/op/embed.aspx?src=' + url" width='100%' height='1000px' frameborder='0'></iframe> -->
            <Form ref="formInline" :model="formInline" :rules="ruleInline" inline>
                  <FormItem prop="user">
                      <Input type="text" v-model="formInline.user" placeholder="Username">
                          <Icon type="ios-person-outline" slot="prepend"></Icon>
                      </Input>
                  </FormItem>
                  <FormItem prop="password">
                      <Input type="password" v-model="formInline.password" placeholder="Password">
                          <Icon type="ios-locked-outline" slot="prepend"></Icon>
                      </Input>
                  </FormItem>
                  <FormItem>
                      <Button type="primary" @click="handleSubmit('formInline')">Signin</Button>
                  </FormItem>
             </Form>

              <form action="/api/user/login" method="post" target="iframe_display">
              user name:<br>
              <input type="text" name="un" value="13110635052">
              <br>
              password:<br>
              <input type="text" name="pw" value="123456">
              <br><br>
              <input type="submit" value="Submit">
              </form> 
              <button @click="getFileList">getFileList</button>

          </div>
        </div>
        <div class="layout-copy">
          2017-2018 &copy; Chen Zhangwei
        </div>
        </Col>
      </Row>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      courses: [],
      schools: [],
      url: "",
      userInfo: [],
      formInline: {
        user: "",
        password: ""
      },
      ruleInline: {
        user: [
          {
            required: true,
            message: "Please fill in the user name",
            trigger: "blur"
          }
        ],
        password: [
          {
            required: true,
            message: "Please fill in the password.",
            trigger: "blur"
          },
          {
            type: "string",
            min: 6,
            message: "The password length cannot be less than 6 bits",
            trigger: "blur"
          }
        ]
      }
    };
  },
  mounted: function() {
    this.$nextTick(function() {
      //初始化完成后执行这个函数
      // this.login();
      // this.autoLogin();
      // this.getFileList();
      // this.get_school_list();
    });
  },
  methods: {
    display: function() {
      this.url = "http://static.finalexam.cn/ppt.ppt";
    },
    get_school_list: function() {
      this.$http.post("http://static.finalexam.cn/schools.json").then(
        response => {
          this.schools = response.body.res;
          console.log(response);
        },
        response => {
          // error callback
        }
      );
    },
    get_course: function(school_num) {
      this.$http
        .post("https://finalexam.cn/api/school/listAcademy", {
          college: school_num
        })
        .then(
          response => {
            this.courses = response.body.res;
          },
          response => {
            // error callback
          }
        );
    },
    login: function() {
      this.$http
        .post("/api/user/login", {
          un: "13110635052",
          pw: "123456",
          credentials: true
        })
        .then(
          response => {
            this.userInfo = response.body.res;
            console.log(response);
          },
          response => {
            // error callback
          }
        );
    },
    getFileList: function() {
      this.$http
        .post("/api/dbfs/1/list", { path: '/', detail: 0})
        .then(response => {
          console.log(response);
        });
    },
    addCookie: function(name, value, days, path) {
      /**添加设置cookie**/

      var name = escape(name);
      var value = escape(value);
      var expires = new Date();
      expires.setTime(expires.getTime() + days * 3600000 * 24);
      //path=/，表示cookie能在整个网站下使用，path=/temp，表示cookie只能在temp目录下使用
      path = path == "" ? "" : ";path=" + path;
      //GMT(Greenwich Mean Time)是格林尼治平时，现在的标准时间，协调世界时是UTC
      //参数days只能是数字型
      var _expires =
        typeof days == "string" ? "" : ";expires=" + expires.toUTCString();
      document.cookie = name + "=" + value + _expires + path;
    },
    autoLogin: function() {
      document.cookie = "un=myCookie4; domain=.google.com.hk; max-age=10800;";
    },
    handleSubmit(name) {
      this.$refs[name].validate(valid => {
        if (valid) {
          this.$Message.success("Success!");
        } else {
          this.$Message.error("Fail!");
        }
      });
    }
  }
};
</script>

<style scoped>
.layout {
  border: 1px solid #d7dde4;
  background: #f5f7f9;
  position: relative;
}
.layout-content {
  min-height: 1000px;
  margin: 15px;
  overflow: hidden;
  background: #fff;
  border-radius: 4px;
}
.layout-content-main {
  padding: 10px;
}
.layout-copy {
  text-align: center;
  padding: 10px 0 20px;
  color: #9ea7b4;
}
.layout-menu-left {
  background: #464c5b;
}
.layout-header {
  height: 60px;
  background: #fff;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.1);
}
.layout-logo-left {
  width: 90%;
  height: 30px;
  background: #5b6270;
  border-radius: 3px;
  margin: 15px auto;
}
</style>

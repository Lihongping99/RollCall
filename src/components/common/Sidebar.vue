<template>
    <div class="sidebar">
        <el-menu :default-active="onRoutes" class="el-menu-vertical-demo" theme="dark" unique-opened router>
            <template v-for="item in items">
                <template v-if="item.subs">
                    <el-submenu :index="item.index">
                        <template slot="title" @click="hh()"><i :class="item.icon"></i>{{ item.title }}</template>
                        <el-menu-item v-for="(subItem,i) in item.subs" :key="i" :index="subItem.index">{{ subItem.title }}
                        </el-menu-item>
                    </el-submenu>
                </template>
                <template v-else>
                    <el-menu-item :index="item.index">
                        <i :class="item.icon"></i>{{ item.title }}
                    </el-menu-item>
                </template>
            </template>
        </el-menu>
    </div>
</template>

<script>
export default {
  data() {
    return {
      items: [
        {
          icon: "el-icon-setting",
          index: "system",
          title: "系统管理",
          subs: [
            {
              index: "addadmin",
              title: "添加管理员"
            },
            {
              index: "manageadmin",
              title: "管理员管理"
            },
            {
              index: "modifypsw",
              title: "修改登录密码  "
            }
          ]
        },
        {
          icon: "el-icon-setting",
          index: "faculty",
          title: "院系班级管理",
          subs: [
            {
              index: "addfaculty",
              title: "添加院系信息"
            },
            {
              index: "managefaculty",
              title: "管理院系信息"
            },
            {
              index: "addclass",
              title: "添加班级信息"
            },
            {
              index: "manageclass",
              title: "管理班级信息"
            }
          ]
        },
        {
          icon: "el-icon-menu",
          index: "rollcall",
          title: "点名信息管理",
          subs: [
            {
              index: "managerollcall",
              title: "点名信息管理"
            }
          ]
        },
        {
          icon: "el-icon-date",
          index: "course",
          title: "课程管理",
          subs: [
            {
              index: "addcourse",
              title: "添加课程信息"
            },
            {
              index: "managecourse",
              title: "管理课程信息"
            }
          ]
        },
        {
          icon: "el-icon-star-on",
          index: "user",
          title: "用户信息管理",
          subs: [
            {
              index: "addstudent",
              title: "添加学生信息"
            },
            {
              index: "managestudent",
              title: "管理学生信息"
            },
            {
              index: "addheadteacher",
              title: "添加班主任信息"
            },
            {
              index: "manageheadteacher",
              title: "管理班主任信息"
            },
            {
              index: "addteacher",
              title: "添加任课教师"
            },
            {
              index: "manageteacher",
              title: "管理任课教师"
            }
          ]
        },
        {
          icon: "el-icon-menu",
          index: "7",
          title: "表格",
          subs: [
            {
              index: "basetable",
              title: "基础表格"
            },
            {
              index: "vuetable",
              title: "Vue表格组件"
            }
          ]
        },
        {
          icon: "el-icon-date",
          index: "4",
          title: "表单",
          subs: [
            {
              index: "baseform",
              title: "基本表单"
            },
            {
              index: "vueeditor",
              title: "编辑器"
            },
            {
              index: "markdown",
              title: "markdown"
            },
            {
              index: "upload",
              title: "文件上传"
            }
          ]
        },
        {
          icon: "el-icon-star-on",
          index: "basecharts",
          title: "图表"
        },
        {
          icon: "el-icon-upload2",
          index: "drag",
          title: "拖拽"
        }
      ]
    };
  },
  computed: {
    onRoutes() {
      return this.$route.path.replace("/", "");
    }
  },
  methods:{
      hh(){
          alert(2)
      }
  }
};
</script>

<style scoped>
.sidebar {
  display: block;
  position: absolute;
  width: 250px;
  left: 0;
  top: 70px;
  bottom: 0;
  background: #2e363f;
  overflow-y: auto;
}
/* 实现隐藏滚动条同时又可以滚动 */
/*  如果要兼容 PC 其他浏览器（IE、Firefox 等），国外一位才人 John Kurlak 也研究出了一种办法。
    在容器外面再嵌套一层 overflow:hidden 内部内容再限制尺寸和外部嵌套层一样，就变相隐藏了。 
    参考 https://www.cnblogs.com/alice626/p/6206760.html  */
.sidebar::-webkit-scrollbar {
  display: none;
}
.sidebar > ul {
  height: 100%;
}
</style>

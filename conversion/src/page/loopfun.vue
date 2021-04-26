<!--  -->
<template>
  <div class="box">
    <p>导出数据</p>
    <a-divider />
    <div class="contextbox">
      <!-- layout="inline" -->
      <a-form-model
        :model="formInline"
        @submit="handleSubmit"
        @submit.native.prevent
        class="formbox"
      >
        <a-form-model-item
          :label-col="formItemLayout.labelCol"
          :wrapper-col="formItemLayout.wrapperCol"
          label="前缀名"
        >
          <a-input
            v-model="formInline.label"
            placeholder="前缀"
            style="width: 300px"
          >
            <a-icon
              slot="prefix"
              type="user"
              style="color: rgba(0, 0, 0, 0.25)"
            />
          </a-input>
        </a-form-model-item>
        <a-form-model-item
          :label-col="formItemLayout.labelCol"
          :wrapper-col="formItemLayout.wrapperCol"
          label="开始值"
        >
          <a-input
            v-model="formInline.user"
            placeholder="start"
            style="width: 300px"
          >
            <a-icon
              slot="prefix"
              type="user"
              style="color: rgba(0, 0, 0, 0.25)"
            />
          </a-input>
        </a-form-model-item>
        <a-form-model-item
          :label-col="formItemLayout.labelCol"
          :wrapper-col="formItemLayout.wrapperCol"
          label="结束值"
        >
          <a-input
            v-model="formInline.password"
            placeholder="end"
            style="width: 300px"
          >
            <a-icon
              slot="prefix"
              type="lock"
              style="color: rgba(0, 0, 0, 0.25)"
            />
          </a-input>
        </a-form-model-item>
        <a-form-model-item
          :label-col="formItemLayout.labelCol"
          :wrapper-col="formItemLayout.wrapperCol"
          label="文件名"
        >
          <a-input
            v-model="formInline.filename"
            placeholder="filename"
            style="width: 300px"
          >
            <a-icon
              slot="prefix"
              type="lock"
              style="color: rgba(0, 0, 0, 0.25)"
            />
            <a-select
              slot="addonAfter"
              default-value=".csv"
              style="width: 80px"
              @change="handleChange"
            >
              <a-select-option value=".csv"> .csv </a-select-option>
              <a-select-option value=".xls"> .xls </a-select-option>
              <a-select-option value=".xlsx"> .xlsx </a-select-option>
            </a-select>
          </a-input>
        </a-form-model-item>
        <a-form-model-item
          :label-col="formTailLayout.labelCol"
          :wrapper-col="formTailLayout.wrapperCol"
        >
          <a-button
            type="primary"
            html-type="submit"
            :disabled="
              formInline.user === '' ||
              formInline.password === '' ||
              formInline.label === ''
            "
          >
            生成数据
          </a-button>
        </a-form-model-item>
      </a-form-model>
    </div>
    <div>
      <!-- <a-button type="primary" @click="two"> 导出1 </a-button> -->
      <!-- <a-button type="primary" @click="Arraylist"> 导出2</a-button> -->
      <!-- <a-button type="primary" @click="files"> 导出3</a-button> -->
    </div>
  </div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';
const formItemLayout = {
  labelCol: { span: 4 },
  wrapperCol: { span: 8 },
};
const formTailLayout = {
  labelCol: { span: 4 },
  wrapperCol: { span: 8, offset: 4 },
};
export default {
  name: "",
  //import引入的组件需要注入到对象中才能使用
  components: {},
  props: [],
  data() {
    //这里存放数据
    return {
      formItemLayout,
      formTailLayout,
      formInline: {
        label: "",
        user: "",
        password: "",
        filename: "数据表",
        sufname: ".csv",
      },
      list: [],
      strdata: "",
    };
  },
  //监听属性 类似于data概念
  computed: {},
  //监控data中的数据变化
  watch: {},
  //方法集合
  methods: {
    //表格类型选项 三种类型
    handleChange(e) {
      this.formInline.sufname = e;
      // console.log(e);
    },
    handleSubmit(e) {
      // console.log(this.formInline);
      this.loopfun();
    },
    loopfun() {
      let v1 = +this.formInline.user;
      let v2 = +this.formInline.password;
      let v3 = this.formInline.label;
      let list = [];

      for (let i = v1; i <= v2; i++) {
        let v4 = v3 + i.toString(16).toLocaleUpperCase();
        // console.log(v4);
        list.push(v4);
      }
      this.list = list;
      // console.log(list);
      this.Arraylist();
    },
    //导出到
    Arraylist() {
      let list = this.list;
      let str = "";
      for (const key in list) {
        str += `${list[key] + "\t"},\n`;
      }
      // console.log("e", str);
      this.strdata = str;
      this.files(str);
    },
    //下载到本地
    files(str) {
      const uri =
        "data:text/csv;charset=utf-8,\ufeff" + encodeURIComponent(this.strdata);
      // 通过创建a标签实现
      const link = document.createElement("a");
      link.href = uri;
      // 对下载的文件命名
      link.download = `${this.formInline.filename}+${this.formInline.sufname}`;
      link.click();
      this.$message.success("操作成功！");
    },
    two() {
      // 要导出的json数据
      const jsonData = [
        {
          name: "路人甲",
          phone: "123456789",
          email: "000@123456.com",
        },
        {
          name: "炮灰乙",
          phone: "123456789",
          email: "000@123456.com",
        },
        {
          name: "土匪丙",
          phone: "123456789",
          email: "000@123456.com",
        },
        {
          name: "流氓丁",
          phone: "123456789",
          email: "000@123456.com",
        },
      ];
      // 列标题，逗号隔开，每一个逗号就是隔开一个单元格
      let str = `姓名,电话,邮箱\n`;
      // 增加\t为了不让表格显示科学计数法或者其他格式
      for (let i = 0; i < jsonData.length; i++) {
        for (const key in jsonData[i]) {
          str += `${jsonData[i][key] + "\t"},`;
        }
        str += "\n";
      }
      // encodeURIComponent解决中文乱码
      const uri =
        "data:text/csv;charset=utf-8,\ufeff" + encodeURIComponent(str);
      // 通过创建a标签实现
      const link = document.createElement("a");
      link.href = uri;
      // 对下载的文件命名
      link.download = "json数据表.csv";
      link.click();
      console.log("6", str);
    },
  },
  //生命周期 - 创建完成（可以访问当前this实例）
  created() {},
  //生命周期 - 挂载完成（可以访问DOM元素）
  mounted() {},
  beforeCreate() {}, //生命周期 - 创建之前
  beforeMount() {}, //生命周期 - 挂载之前
  beforeUpdate() {}, //生命周期 - 更新之前
  updated() {}, //生命周期 - 更新之后
  beforeDestroy() {}, //生命周期 - 销毁之前
  destroyed() {}, //生命周期 - 销毁完成
  activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发
};
</script>
<style lang='scss' scoped>
//@import url(); 引入公共css类
.contextbox {
  height: 100%;
  justify-content: center;
  align-items: center;
  display: flex;
  flex-direction: column;
}
.box {
  margin: 0 auto;
  height: 100%;
}
.formbox {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
</style>

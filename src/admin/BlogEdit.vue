<template>
  <div>
    <!--主体内容-->
    <div class="m-content">
      <!--博客内容表单-->
      <el-form ref="ruleForm" :model="ruleForm" :rules="rules" class="demo-ruleForm" label-position="top"
               label-width="100px">
        <!--文章标题和首图url-->
        <el-row :gutter="20">
          <el-col :span="12">
            <el-form-item label="文章标题" prop="title">
              <el-input v-model="ruleForm.title" placeholder="请输入标题"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="文章首图URL" prop="firstPicture">
              <el-input v-model="ruleForm.firstPicture" placeholder="文章首图，可为空"></el-input>
            </el-form-item>
          </el-col>
        </el-row>

<!--        &lt;!&ndash;生成的文本&ndash;&gt;-->
<!--        <el-form-item label="文本生成输入" prop="description">-->
<!--          <el-input v-model="ruleForm.textNeedGen" placeholder="请输入需要生成的文本"></el-input>-->
<!--        </el-form-item>-->
<!--        <el-form-item label="文本生成结果" prop="description">-->
<!--          <el-input v-model="ruleForm.textGen"  :disabled="true"></el-input>-->
<!--        </el-form-item>-->

<!--        &lt;!&ndash;文章描述&ndash;&gt;-->
<!--        <el-form-item label="文章描述" prop="description">-->
<!--          <mavon-editor @dblclick.native="autoWrite" v-model="ruleForm.description"/>-->
<!--        </el-form-item>-->
        <el-form-item label="文章正文" prop="content">
          <mavon-editor  v-model="ruleForm.content"></mavon-editor>
        </el-form-item>


        <!--文本生成-->

        <el-row :gutter="100">
          <el-col :span="12">
            <el-form-item label="文本生成输入" prop="title">
              <el-input
                      type="textarea"
                      :rows="5"
                      placeholder="文本生成输入"
                      v-model="textGenForm.textNeedGen">
              </el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="文本生成输出" prop="title">
              <el-input
                      type="textarea"
                      :rows="5"
                      placeholder="文本生成输出"
                      v-model="textGenForm.textGen">
              </el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-form-item>
          <el-button @click="textGen()">文本生成</el-button>
        </el-form-item>

        <!--图片生成-->
        <el-form-item label="图片生成输入" prop="title">
          <el-input
                  type="textarea"
                  :rows="3"
                  placeholder="图片生成输入"
                  v-model="pictrueGenForm.textNeedGen">
          </el-input>
        </el-form-item>
        <el-form-item label="图片生成结果" prop="description">
          <mavon-editor  v-model="pictrueGenForm.pictureURLs"/>
        </el-form-item>

        <el-form-item>
          <el-button @click="pictureGen()">图片生成</el-button>
        </el-form-item>


<!--        <el-row :gutter="100">-->
<!--          <el-col :span="12">-->
<!--            <el-form-item label="文本生成输入" prop="title">-->
<!--              <el-input type="textarea" v-model="ruleForm.textNeedGen" placeholder="请输入需要生成的文本" size=""></el-input>-->
<!--            </el-form-item>-->
<!--          </el-col>-->

<!--          <el-col :span="12">-->
<!--            <el-form-item label="文本生成结果" prop="firstPicture">-->
<!--              <el-input type="textarea" v-model="ruleForm.textGen"  :disabled="true" ></el-input>-->
<!--            </el-form-item>-->
<!--          </el-col>-->
<!--        </el-row>-->


        <!--分类-->
        <el-row :gutter="20">
          <el-col :span="12">
            <el-form-item label="分类" prop="cate">
              <el-select v-model="ruleForm.typeId" :allow-create="true" :filterable="true" placeholder="请选择分类（输入可添加新分类）" style="width: 100%;">
                <el-option v-for="type in types" :key="type.id" :label="type.typeName" :value="type.id"></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <!--标签，暂时未实现-->
          <el-col :span="12">
            <el-form-item label="标签" prop="tagList">
              <el-select v-model="ruleForm.tagList" :allow-create="true" :filterable="true" :multiple="true" placeholder="请选择标签（输入可添加新标签）" style="width: 100%;">
                <el-option v-for="item in tagList" :key="item.id" :label="item.name" :value="item.id"></el-option>
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
        <!--字数显示-->
        <el-row :gutter="20">
          <el-col :span="12">
            <el-form-item label="字数" prop="words">
              <!-- 双击计算文章字数-->
              <el-input v-model="ruleForm.words" placeholder="请输入文章字数" type="number" @dblclick.native="computeWords"></el-input>
            </el-form-item>
          </el-col>
          <!--浏览次数-->
          <el-col :span="12">
            <el-form-item label="浏览次数" prop="views">
              <el-input v-model="ruleForm.views" placeholder="请输入文章字数（可选）默认为 0" type="number"></el-input>
            </el-form-item>
          </el-col>
          <!--是否公开-->
          <el-col :span="12">
            <el-form-item label="是否公开" prop="publised">
              <el-tooltip :content="'Switch value: ' + ruleForm.status" placement="top">
                <el-switch v-model="ruleForm.status" :active-value="1" :inactive-value="0"></el-switch>
              </el-tooltip>
            </el-form-item>
          </el-col>
        </el-row>
        <!--提交和重置按钮-->
        <el-form-item>
          <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
          <el-button @click="resetForm('ruleForm')">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: "BlogEdit",
  data() {
    return {
      types: {},
      ruleForm: {
        id: '',
        title: '',
        description: 'meiyouzhaiyao',
        content: '',
        firstPicture: '',
        typeId: '',
        createTime: '2020-05-22 22:05:49',
        updateTime: '2020-05-22 22:05:49',
        userId: 1,
        words: null,
        views: 0,
        status: 0,
      },
      rules: {
        title: [
          {required: true, message: '请输入标题', trigger: 'blur'},
          {min: 2, max: 45, message: '长度在 3 到 45 个字符', trigger: 'blur'}
        ],
        description: [
          {required: false, message: '请输入摘要', trigger: 'blur'}
        ],
        content: [
          {required: true, message: '请输入内容', trigger: 'blur'}
        ],
        words: [{required: true, message: '请输入文章字数', trigger: 'change'}],
      },
      textGenForm: {
        textNeedGen: '',
        textGen: '',
      },
      pictrueGenForm: {
        textNeedGen: '',
        pictureURLs: '',
      }
    }
  },
  methods: {
    //双击将正文的内容前100字自动写入文章描述中
    autoWrite() {
      if(this.ruleForm.content.length<100){
        this.ruleForm.description = this.ruleForm.content.substring(0,this.ruleForm.content.length)
      }
      else{
        this.ruleForm.description = this.ruleForm.content.substring(0,100)
      }
    },
    //双击将计算正文内容的字数
    computeWords() {
      var contents = this.ruleForm.content
      this.ruleForm.words = contents.replace(/^[\s\uFEFF\xA0]+|[\s\uFEFF\xA0]+$/g, '').length
    },
    //提交博客
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          const _this = this
          if(_this.ruleForm.id==""){
            this.$axios.post('/blog/create', this.ruleForm).then(res => {
              console.log(res)
              _this.$alert('操作成功', '提示', {
                confirmButtonText: '确定',
                callback: action => {
                  _this.$router.push("/blogList")
                }
              });
            })
          }
          else{
            this.$axios.post('/blog/update', this.ruleForm).then(res => {
              console.log(res)
              _this.$alert('操作成功', '提示', {
                confirmButtonText: '确定',
                callback: action => {
                  _this.$router.push("/blogList")
                }
              });
            })
          }

        }
        else {
          console.log('error submit!!');
          return false;
        }
      });
    },
    //重置博客内容
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
    //获取博客分类
    getTypes() {
      const _this = this
      this.$axios.get('/types').then(res => {
        _this.types = res.data.data
      })
      console.log(this.types)
    },
    //通过id获取博客
    getBlogById() {
      const blogId = this.$route.params.blogId
      const _this = this
      if (blogId) {
        this.$axios.get('/blog/detail/' + blogId).then(res => {
          const blog = res.data.data
          _this.ruleForm.id = blog.id
          _this.ruleForm.title = blog.title
          _this.ruleForm.firstPicture = blog.firstPicture
          _this.ruleForm.description = blog.description
          _this.ruleForm.content = blog.content
          _this.ruleForm.words = blog.words
          _this.ruleForm.views = blog.views
          _this.ruleForm.typeId = blog.typeId
          _this.ruleForm.status = blog.status
          _this.ruleForm.userId = blog.userId
          _this.ruleForm.createTime = blog.createTime
          _this.ruleForm.updateTime = blog.updateTime
        })
      }
    },
    textGen() {
      this.textGenForm.textNeedGen = "首批加持骁龙8移动平台的安卓旗舰手机都已经相继问世了，前有摩托罗拉后有小米，要说表现最亮眼的还是由刘作虎带领的OPPO新团队打造的开年旗舰手机一加10 Pro和OPPO Find N折叠屏手机，首批用户的评价出炉，获得了口碑和销量的双赢战绩。"
      this.textGenForm.textGen = "用心打磨的好产品市场一定是能看得见的，OPPO旗下由刘作虎带领的新团队开年带来的两款旗舰产品赢得了口碑和销量的双赢好战绩，其中一加10 Pro开售1秒销售额就破亿，OPPO Find N折叠屏手机更不用说了直接是预约破百万的记录。"
    },

    pictureGen() {
      this.pictrueGenForm.textNeedGen = "城市风光"
      //![asdff](https://t7.baidu.com/it/u=2259261418,2370578071&fm=193&f=GIF)
      this.pictrueGenForm.pictureURLs = "![1](https://t7.baidu.com/it/u=2259261418,2370578071&fm=193&f=GIF)"
    },
  },
  created() {
    this.getTypes();
    this.getBlogById();
  }
}
</script>
<style scoped>
.m-content {
  width: 100%;
}
</style>
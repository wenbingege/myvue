<template>
  <div>
    <!-- 查询form -->
      <el-form :model="searchForm" ref="searchForm" class="demo-form-inline"  label-width="100px" size="medium" >
        <el-row>
          <el-col :span="8">
            <el-form-item label="姓名" prop="personName">
              <el-input
                v-model="searchForm.personName"
                class="search_name"
                placeholder="输入姓名查询" style="width: 70%;" >
              </el-input>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="个人定位" prop="personAttr">
              <el-select v-model="searchForm.personAttr">
                <el-option label="技术型" value="技术型"></el-option>
                <el-option label="业务型" value="业务型"></el-option>
                <el-option label="营销型" value="营销型"></el-option>
                <el-option label="管理型" value="管理型"></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="入职日期">
              <el-date-picker
                    v-model="searchForm.personDate"
                    align="right"
                    type="date"
                    value-format="yyyy-MM-dd HH:mm:ss"
                    placeholder="选择日期"
                    :picker-options="pickerOptions1">
              </el-date-picker>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="8">
            <el-form-item label="省" prop="province">
              <el-select @change="selectChanged" v-model="searchForm.province" placeholder="请选择">
                <el-option
                  v-for="item in provinces"
                  :key="item.cityNum"
                  :label="item.cityName"
                  :value="item.cityNum">
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="市" prop="city">
              <el-select @change="selectChanged" v-model="searchForm.city" placeholder="请选择">
                <el-option
                  v-for="item in citys"
                  :key="item.cityNum"
                  :label="item.cityName"
                  :value="item.cityNum">
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="区" prop="regional">
              <el-select v-model="searchForm.regional" placeholder="请选择">
                <el-option
                  v-for="item in regionals"
                  :key="item.cityNum"
                  :label="item.cityName"
                  :value="item.cityNum">
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :push="16">
            <el-button
              type="info"
              @click="onSearch()"
              class="el-icon-search">查询
            </el-button>
            <el-button
              class="el-icon-refresh"
              type="info"
              @click="reset('searchForm')">重置
            </el-button>
            <el-button
              class="el-icon-circle-plus-outline"
              type="info"
              @click="dialogVisible = true">添加
            </el-button>
          </el-col>
        </el-row>
      </el-form>
<!-- 数据table -->
      <el-table
        :data="tableData"
        highlight-current-row
        border
        style="width: 100%">
        <el-table-column
          label="姓名">
          <template slot-scope="scope">
            <span>{{ scope.row.personName }}</span>
          </template>
        </el-table-column>
        <el-table-column
          label="性别">
          <template slot-scope="scope">
            <span>{{ scope.row.personSex }}</span>
          </template>
        </el-table-column>
        <el-table-column
          label="年龄">
          <template slot-scope="scope">
            <span>{{ scope.row.personAge }}</span>
          </template>
        </el-table-column>
        <el-table-column
          label="入职日期">
          <template slot-scope="scope">
            <i class="el-icon-time hidden-sm-and-down"></i>
            <span>{{ scope.row.personDate }}</span>
          </template>
        </el-table-column>
        <el-table-column
          label="个人规划">
          <template slot-scope="scope">
            <span>{{ scope.row.personPlan }}</span>
          </template>
        </el-table-column>
        <el-table-column
          label="个人定位">
          <template slot-scope="scope">
            <span>{{ scope.row.personAttr }}</span>
          </template>
        </el-table-column>
      </el-table>
<!-- 增加 -->
      <el-form :model="addForm" :rules="rules" ref="addForm" label-position="right" label-width="100px" class="demo-ruleForm" size="medium">
        <el-dialog
          title="基础信息"
          :append-to-body='true'
          :visible.sync="dialogVisible"
          width="70%"
          :before-close="handleClose">
          <el-input type="hidden" v-model="addForm.personName"/>
          <el-row>
            <el-col :span="8">
              <el-form-item label="姓名" prop="personName">
                <el-input v-model="addForm.personName"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="性别" prop="personSex">
                <el-select v-model="addForm.personSex" placeholder="请选择性别" style="width: 100%;">
                  <el-option label="男" value="男"></el-option>
                  <el-option label="女" value="女"></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="年龄" prop="personAge">
                <el-input v-model="addForm.personAge"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="8">
              <el-form-item label="入职日期" prop="personDate">
                <el-date-picker type="datetime" placeholder="选择日期" v-model="addForm.personDate" value-format="yyyy-MM-dd HH:mm:ss" :picker-options="pickerOptions1" style="width: 100%;"></el-date-picker>
              </el-form-item>
            </el-col>
            <el-col span="8">
              <el-form-item label="邮箱" prop="personEmail">
                <el-input v-model="addForm.personEmail"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="联系方式" prop="personPhone">
                <el-input v-model="addForm.personPhone"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="24">
              <el-form-item label="个人规划" prop="personPlan">
                <el-input v-model="addForm.personPlan"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="24">
              <el-form-item label="自我定位" prop="personAttr">
                <el-checkbox-group v-model="addForm.personAttr">
                  <el-checkbox label="技术型" ></el-checkbox>
                  <el-checkbox label="业务型" ></el-checkbox>
                  <el-checkbox label="营销型" ></el-checkbox>
                  <el-checkbox label="管理型" ></el-checkbox>
                </el-checkbox-group>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="24">
              <el-form-item label="公司所在地址"  >
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="8">
              <el-form-item label="省">
                <el-select @change="selectChanged" v-model="addForm.province" placeholder="请选择">
                  <el-option
                    v-for="item in provinces"
                    :key="item.cityNum"
                    :label="item.cityName"
                    :value="item.cityNum">
                  </el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="市">
                <el-select @change="selectChanged" v-model="addForm.city" placeholder="请选择">
                  <el-option
                    v-for="item in citys"
                    :key="item.cityNum"
                    :label="item.cityName"
                    :value="item.cityNum">
                  </el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="区">
                <el-select v-model="addForm.regional" placeholder="请选择">
                  <el-option
                    v-for="item in regionals"
                    :key="item.cityNum"
                    :label="item.cityName"
                    :value="item.cityNum">
                  </el-option>
                </el-select>
              </el-form-item>
            </el-col>
          </el-row>
          <el-form-item>
            <center>
              <el-button @click="cancel()" size="medium">关 闭</el-button>
              <el-button @click="addPerson('addForm')" type="primary" size="medium">保 存</el-button>
            </center>
          </el-form-item>
        </el-dialog>
      </el-form>
      <br>
      <!-- 翻页组件 -->
      <div class="pages">
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="currentPage"
          :page-sizes="[5, 10, 15, 20]"
          :page-size="pageSize"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total">
        </el-pagination>
      </div>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                //添加form
                addForm: {
                    personName: '',//姓名
                    personSex: '',//性别
                    personAge: '',//年龄
                    personDate: '',//入职日期
                    personEmail: '',//邮箱
                    personPhone: '',//联系方式
                    personPlan: '',//个人规划
                    personAttr: [],//个人定位
                    province: '',//省
                    city: '',//市
                    regional: ''//区
                },
                rules: {
                    personName: [
                        { required: true, message: '请输入姓名', trigger: 'blur' },
                        { min: 2, max: 7, message: '长度在 2 到 7 个字符', trigger: 'blur' }
                    ],
                    personAge: [
                        { required: false, message: '请输入年龄', trigger: 'blur' },
                        { type: 'number', message: '请输入数字类型', trigger: ['blur','change'] }
                    ],
                    personEmail: [
                        { required: true, message: '请输入邮箱地址', trigger: 'blur' },
                        { type: 'email', message: '请输入正确的邮箱地址', trigger: ['blur', 'change'] }
                    ],
                    personPhone: [
                        { required: true, message: '请输入手机号', trigger: 'blur' },
                        { pattern: /^((0\d{2,3}-\d{7,8})|(1[3584]\d{9}))$/, message: '请输入正确的手机号码或者座机号', }
                    ],
                },
                //时间选择
                pickerOptions1: {
                    disabledDate(time) {
                        return time.getTime() > Date.now();
                    }
                },
                //报表数据集合
                tableData: [],
                search: '',
                dialogVisible: false,
                pageSize: 5,
                currentPage: 1,
                total: 0,
                //查询form
                searchForm: {
                  personName: '',//用户姓名
                  personAttr: '',//个人定位
                  personDate: '',//入职时间
                  province: '',//省
                  city: '',//市
                  regional: ''//区
                },
                disablePage: false,
                provinces: [],//省
                citys: [],//市
                regionals: []//区
            }
        },
        methods: {
            handleSizeChange(val) {
              console.log(`每页 ${val} 条`);
              this.pageSize=val;
            },
            handleCurrentChange(val) {
                console.log(`当前页: ${val}`);
                this.currentPage=val;
                this.onSearch();
            },
            //增加页面关闭事件
            handleClose(done) {
                this.$confirm('确认关闭？')
                    .then(_ => {
                        done();
                    })
                    .catch(_ => {});
            },
            //增加页面取消事件
            cancel() {
                this.dialogVisible = false;
                this.emptyUserData();
            },
            emptyUserData(){
                this.addForm = {
                    personName: '',//姓名
                    personSex: '',//性别
                    personAge: '',//年龄
                    personDate: '',//入职日期
                    personEmail: '',//邮箱
                    personPhone: '',//联系方式
                    personPlan: '',//个人规划
                    personAttr: '',//个人定位
                    province: '',//省
                    city: '',//市
                    regional: ''//区
                }
            },
            //增加用户与服务器交互
            addPerson(addForm) {
               this.$refs[addForm].validate((valid) => {
                 if (valid) {
                   this.addPersonSubmit();
                   this.dialogVisible = false;
                 } else {
                   console.log('error submit!!');
                   return false;
                 }
               });
            },
            //form提交事件
            addPersonSubmit() {
              let postData = this.qs.stringify({
                  personName: this.addForm.personName,//姓名
                  personSex: this.addForm.personSex,//性别
                  personAge: this.addForm.personAge,//年龄
                  personDate: this.addForm.personDate,//入职日期
                  personEmail: this.addForm.personEmail,//邮箱
                  personPhone: this.addForm.personPhone,//联系方式
                  personPlan: this.addForm.personPlan,//个人规划
                  personAttr: this.addForm.personAttr.join(","),//个人定位
                  province: this.addForm.province,//省
                  city: this.addForm.city,//市
                  regional: this.addForm.regional
              });
              this.axios({
                  // headers: {
                  //     'Content-Type': 'application/json;charset=utf-8'
                  // },
                  method: 'post',
                  //http://127.0.0.1:8080
                  url:'/api/person/save',
                  data:postData
              }).then(response =>
              {
                  var code = response.data.code;
                  if(code == 200){
                    this.currentPage = 1;
                    this.$message({
                        type: 'success',
                        message: '已添加!'
                    });
                    this.onSearch();
                  }else{
                    this.$message({
                        type: 'error',
                        message: response.data.msg
                    });
                  }
              }).catch(error =>
              {
                  console.log(error);
              });

            },
            //获取所属城市
            selectCitys(cityParentNum,cityLeave) {
              //http://127.0.0.1:8080
                this.axios.get('/api/city/findList',{
                  params: {
                        cityParentNum: cityParentNum,
                        cityLeave: cityLeave,
                        }
                }).then(response =>
                {
                    console.log(response.data);
                    let data = response.data.data;
                    //判断城市等级，将对应的数据放到对应的集合，同时初始化子城市
                    if(cityLeave == '1'){
                      this.provinces = data;
                    }else if(cityLeave == '2'){
                      this.searchForm.city = '';
                      this.citys = data;
                      this.searchForm.regional = '';
                    }else{
                      this.searchForm.regional = '';
                      this.regionals = data;
                    }
                }).catch(error =>
                {
                    console.log(error);
                });
            },
            //省市区级联选中事件
            selectChanged(cityNum) {
              console.log(cityNum);
              var cityLeave = '2';
              console.log('error'+cityNum.length);
              if(cityNum.length > 4){
                cityLeave = '3';
              }
              this.selectCitys(cityNum,cityLeave);
            },
            //查询按钮
            onSearch() {
                let postData = this.qs.stringify({
                    personName: this.searchForm.personName,
                    personAttr: this.searchForm.personAttr,
                    personDate: this.searchForm.personDate,
                    province: this.searchForm.province,
                    city: this.searchForm.city,
                    regional: this.searchForm.regional,
                    pageNum: this.currentPage,
                    pageSize: this.pageSize,
                });
                console.log("postData:"+postData);
                this.axios({
                    method: 'post',
                    //http://127.0.0.1:8080
                    url: '/api/person/findList',
                    data: postData
                }).then(response =>
                {
                    console.log("person/findList:"+response);
                    this.tableData = response.data.rows;
                    this.disablePage = true;
                    this.total = response.data.total;
                }).catch(error =>
                {
                    console.log(error);
                });
            },

            //重置
            reset(searchForm) {
              this.$refs[searchForm].resetFields()
            }
        },
        created() {
            this.onSearch();
            this.selectCitys('0','1');
        },
    }
</script>
<style scoped>
  .search_name{
    width: 200px;
  }
  .pages{
    margin: 0px;
    padding: 0px;
    text-align: right;
  }
</style>

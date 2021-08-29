<template>
  <div>
    <el-form :model="form" ref="totalForm" :rules="form.rules" label-width="120px">
      <el-form-item label="프로젝트 명" prop="name">
        <el-input v-model="form.name"></el-input>
      </el-form-item>
      <el-form-item label="내용" prop="desc">
        <el-input type="textarea" v-model="form.desc"></el-input>
      </el-form-item>

      <el-form-item label="상태" prop="status">
        <el-select v-model="form.status" placeholder="please select your zone">
          <el-option label="Zone one" value="shanghai"></el-option>
          <el-option label="Zone two" value="beijing"></el-option>
        </el-select>
      </el-form-item>

      <el-form-item label="이미지 url" prop="img">
        <el-input v-model="form.img"></el-input>
      </el-form-item>


      <el-form-item label="참가자" prop="selectedMember">

        <el-autocomplete
            v-model="searchResult"
            class="inline-input"
            placeholder="검색어를 입력하세요"
            :trigger-on-focus="false"
            :fetch-suggestions="querySearch"
            @select="handleSelect"
        >
          <template #default="{ item }">
            <div class="value">{{ item.value }}</div>
          </template>
        </el-autocomplete>

        <div>
          <el-tag v-for="item in form.selectedMember" :key="item" closable @close="handleClose(item)">
            {{ item }}
          </el-tag>
        </div>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="create('totalForm')">Create</el-button>
        <el-button>Cancel</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted, reactive } from 'vue'

export default defineComponent({
  setup() {
    const form = reactive({
      name: "",
      status: "",
      img: "",
      desc: "",
      searchMember: "",
      selectedMember: [],
      rules: {
        name: [{ required: true, message: "프로젝트 이름을 넣어주세요", trigger: "blur" }],
        desc: [{ message: "프로젝트 설명을 입력해주세요", trigger: "blur" }],
        status: [{ required: true, message: "프로젝트의 상태를 입력해주세요", trigger: "blur" }],
        img: [{ message: "프로젝트 관련 이미지를 넣어주세요", trigger: "blur" }],
        selectedMember: [{ type: "array", required: true, min: 1, message: "1명 이상 멤버를 선택해주세요" }],
      },
    });

    const handleClose = (tag) => form.selectedMember.splice(form.selectedMember.indexOf(tag), 1);

    const restaurants = ref([]);

    const loadAll = () => {
      return [
        { "value": "김다찬" },
        { "value": "원계신" },
        { "value": "박소정" },
        { "value": "이예진" },
        { "value": "최문선" },
        { "value": "김다나" },
        { "value": "현재현" },
        { "value": "오종철" },
        { "value": "김태훈" }
      ];
    };

    onMounted(() => {restaurants.value = loadAll();})

    const serchFn = () => {

      const searchResult = ref('')
      const querySearch = (queryString, cb) => {
        const results = queryString
            ? restaurants.value.filter(createFilter(queryString))
            : restaurants.value;
        // call callback function to return suggestions
        cb(results);
      };
      const createFilter = (queryString) => {
        return (restaurant) => {
          return (
              restaurant.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0
          );
        };
      };

      const handleSelect = (selected) => {
        form.selectedMember.push(selected.value);
        searchResult.value = ''
      };
      return {
        searchResult, querySearch, createFilter, handleSelect
      }
    }

    const { searchResult, querySearch, createFilter, handleSelect } = serchFn()

    const totalForm = ref('')

      onMounted(() => (console.log(totalForm.value)))

    const create = (formName)=> {
      console.log(totalForm.value)
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('submit!');
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    }

    return {
      form,
      create,
      querySearch,
      handleSelect,
      handleClose,

      searchResult,
      restaurants,
      createFilter,
      loadAll,
    };
  },
});
</script>

<style scoped>

</style>

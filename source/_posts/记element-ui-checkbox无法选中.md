---
title: 记element-ui checkbox无法选中
tags: []
categories: []
toc: false
description: element-ui在某些情况下无法选中，本文记录一些自己遇到的问题
date: 2020-04-09 15:08:17
---

element-ui在某些情况下无法选中，本文记录一些自己遇到的问题

```html
<el-form-item label='选项'>
    <el-checkbox-group v-model="form.type">		
    <el-checkbox v-for="(item,index) in types" :key="index" :label="item.name"  :value="item.value" name="type">
    </el-checkbox>
</el-checkbox-group>
</el-form-item>

<script>
export default{
  data(){
    return {
       types:[{name:'往期',value:'history'},{name:'VIP',value:'vip'},{name:'推荐',value:'recommend'},{name:'顶部',value:'top'}],		
    }
  },
  methods:{
  	 async getCourse(){
                const {data:{code,msg,book}} = await getCourse(this.id);
		
		//可以
                book.type = [];
                this.form = book;

		//错误
		//this.form = book;
                //this.form.type = [];
            }
  }
}


</script>
```
<template>
  <div>
      <button class="btn btn-primary pull-right" data-toggle="modal" data-target="#createProductModal"><i class="fa fa-plus"></i> New Product</button>

      <div class="modal fade" id="createProductModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-md" role="document">
        <div class="modal-content">
          <div class="modal-header bg-primary">
            <h5 class="modal-title" id="exampleModalLabel">Add Product</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true" class="text-white">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <span v-if="errorMessage !== null" class="text-danger text-center">
                <label><b>Opps! </b>{{errorMessage}}</label>
            </span>
            <br v-if="errorMessage !== null">
            <br>
            <div class="form-group">
              <label for="exampleInputEmail1">Title</label>
              <input type="text" class="form-control" v-model="newProduct.title" placeholder="Type title here...">
            </div>
            <div class="form-group">
              <label for="exampleInputEmail1">Description</label>
              <textarea class="form-control" v-model="newProduct.description" placeholder="Type description here..." rows="5">
              </textarea> 
            </div>

            <div class="form-group">
              <label for="exampleInputEmail1">SKU</label>
              <input type="text" class="form-control" v-model="newProduct.sku" placeholder="Type sku here...">
            </div>

            <div class="form-group">
              <label for="exampleInputEmail1">Status</label>
              <select v-model="newProduct.status" class="form-control">
                <option value="not_verified">No need for verification</option>
                <option value="verified">Need verification</option>
              </select>
            </div>

          </div>
        </div>
        <div class="modal-footer">
            <button type="button" class="btn btn-primary" @click="submit()">Submit</button>
        </div>
        </div>
      </div>
    </div>


  </div>
</template>

<script>
import ROUTER from '../../router'
import AUTH from '../../services/auth'
import CONFIG from '../../config.js'
import axios from 'axios'
export default {
  mounted(){
  },
  data(){
    return {
      user: AUTH.user,
      config: CONFIG,
      newProduct: {
        account_id: null,
        title: null,
        description: null,
        sku: null,
        status: 'not_verified'
      },
      errorMessage: null
    }
  },
  props: ['params'],
  methods: {
    redirect(parameter){
      ROUTER.push(parameter)
    },
    submit(){
      if(this.validate()){
        this.newProduct.account_id = this.user.userID
        this.APIRequest('/products/create', this.newProduct).then(response => {
          console.log(response)
        })
      }
    },
    validate(){
      let i = this.newProduct
      if((i.title !== '' || i.title !== null)){
        this.errorMessage = null
        return true
      }else{
        this.errorMessage = 'Please fill in all required fields.'
        return false
      }
    }
  }
}
</script>
<style scoped>
.featured-image{
  width: 100%;
  float: left;
  height: 200px;
  margin-bottom: 10px;
}

.featured-image .options{
  width: 100%;
  float: left;
  text-align: center;
  height: 200px;
  border: solid 1px #ddd;
  overflow-y: hidden;
}
.options input{
  display: none;
}
.options:hover{
  cursor: pointer;
}
.options i{
  font-size: 40px;
  width: 100%;
  float: left;
  margin-top: 75px;
}

.options label{
  width: 100%;
  float: left;
}
.options img{
  width: 100%;
  float: left;
  height: auto;
}
</style>

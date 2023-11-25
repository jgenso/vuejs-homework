<script>
export default {
  name: 'DeleteModal',
  props: {
    visible: Boolean,
    variant: String,
    deleteFn: Function,
    closeFn: Function,
  },
  setup(props) {
    console.log('Props', props)
  },
  data(){
    return{
       OpenClose: this.visible
    }
  },
  methods:{
        OpenCloseFun() {
           this.OpenClose = false
           this.closeFn()
        },
        delete() {
          this.deleteFn(item.id)
        }
  },
  watch: { 
      visible: function(newVal, oldVal) { // watch it
        this.OpenClose = newVal;
        console.log('new' +newVal+ '==' +oldVal)
      }
    }

}
</script>
<template>
    <div >
  <!-- Modal -->
  <div v-if="OpenClose" class="modal fade show" 
   tabindex="-1" aria-labelledby="exampleModalLabel" aria-modal="true" role="dialog" 
   style="display:block">
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header">
          
          <button type="button" @click="OpenCloseFun()" class="btn-close" ></button>
        </div>
        <div class="modal-body">
         <slot></slot>
        </div>
        <div class="modal-footer">
          <button type="button"  @click="deleteFn()" :class="'btn btn-'+variant" >Delete</button>
          <button type="button"  @click="OpenCloseFun()" class="btn btn-secondary" >Cancel</button>
        </div>
      </div>
    </div>
  </div>
  </div>
</template>
  
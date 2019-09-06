<!--
	TODO: Send request first to make sure it's working
	TODO: Split it in 3 steps
	TODO: Fix responsiveness
	TODO: Form validation
-->
<template>
<div ref="fileform" class="upload__drag-and-drop-area">
	<div class="upload__text">
		<input ref="uploadfile" type="file" class="upload__hidden" @change="setFile()"/>

		<!-- no file selected -->
		<div class="upload__options" v-if="file == ''" @click.stop.prevent="browseFiles()">
			<strong>Drop your file here</strong> or click me!
		</div>

		<!-- file is selected -->
		<div class="upload__options" v-else>
			<div class="upload__file-name">{{ file.name }}</div>
			<button class="upload__remove" @click="remove()">
				<i class="material-icons">delete</i>
			</button>
		</div>
	</div>
</div>
</template>


<script>
  export default {
      data(){
          return {
              dragAndDropSupported: false,
              file: ''
          }
	  },
	  watch:{
		  file(file){
			  if(file == undefined) return

			  this.$emit('selectedFile', file)
		  }
	  },
      mounted(){
        // check if drag and drop is supported
        this.dragAndDropSupported = this.isDragAndDropSupported()

        // if so, bind events to element
        if(this.dragAndDropSupported){

            // listen to all events
            var events = ['drag', 'dragstart', 'dragend', 'dragover', 'dragenter', 'dragleave', 'drop']

            // bind an event listener for each
			events.forEach(evt => {
				
				// make sure default (pop up `Choose your file` window) is stopped
				this.$refs.fileform.addEventListener(evt, (e) => {
					e.preventDefault()
					e.stopPropagation()
				})

			})
            
            // get the file from drop event and store it to local file variable
            // reject if file type is not supported			
			this.$refs.fileform.addEventListener('drop', e => {
				var fileName = e.dataTransfer.files[0].name
				
				if(!this.isFileFormatSupported(fileName)) return
				
                this.file = e.dataTransfer.files[0]
			})

        }
      },
      methods:{
          /**
           * Check if drag and drop is supported for your browser
           */
          isDragAndDropSupported(){

            // temporary element to validate if events are prent to do drag n drop
            var div = document.createElement('div')

            // validate if events 'draggable', 'ondragstart', and 'ondrop' elements are present
            // validate if windos has 'FormData' and 'FileReader' present
            return  (('draggable' in div) || ('ondragstart' in div && 'ondrop' in div))
                    && 'FormData' in window
                    && 'FileReader' in window
          },

          /**
           * Check if uploaded file is in the correct format
           */
          isFileFormatSupported(fileName){
			  var supported = /\.(docx?|html?|odt|pdf|xlsx?|ods|pptx?|txt)$/i.test(fileName) 
			  if(!supported){
				alert("File format not supported.")
				return false
			  }
			  return true
          },

          /** 
           * Remove the selected file from the form
           */
          remove(){
              this.file = []
		  },

		  /**
		   * Set selected file on fallback method
		   */
		  setFile(){
			  var fileName = this.$refs.uploadfile.files[0].name
			  if(!this.isFileFormatSupported(fileName)) return
			  this.file = this.$refs.uploadfile.files[0]
		  },
		  /**
		   * Let user browser files instead of drag and drop
		   */
		  browseFiles(){
			  this.$refs.uploadfile.click()
		  }

      }
  }
</script>

<style scoped>
.upload__drag-and-drop-area{
	@apply h-20 bg-gray-100 p-2 rounded w-full;
}
.upload__text{
	@apply justify-center flex relative bg-gray-300 w-full h-full text-black rounded p-1 border border-dashed border-gray-500 border-2 text-gray-600;
}
.upload__text:hover{
	@apply bg-gray-400 cursor-pointer;
}
.upload__hidden{
	@apply hidden;
}
.upload__options{
	@apply m-auto;
}
.upload__file-name{
	@apply text-black font-bold text-sm;
}
.upload__remove{
	@apply absolute top-0 right-0 text-red-800 mt-1 cursor-pointer;
}
</style>
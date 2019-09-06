<template>
  <div class="content">
	<!-- logo -->
	<Logo/>

    <!-- steps -->
	<div id="steps">
		<div class="step step-active"><span>You</span></div>
		<div class="step"><span>CV</span></div>
		<div class="step"><span>Done</span></div>
	</div>

    <!-- form -->
    <form class="myform">
		
		<label class="myform__label" for="firstName">
			First Name<span class="myform__required">*</span>
			<input class="myform__input" v-model="form.first_name" id="firstName" type="text">
		</label>

		<label class="myform__label" for="lastName">
			Last name
			<input class="myform__input" v-model="form.last_name" id="lastName" type="text">
		</label>
		
		<label class="myform__label" for="email">
			Email<span class="myform__required">*</span>
			<input class="myform__input" v-model="form.email" id="email" type="email">
		</label>
		
		<label class="myform__label" for="phone_number">
			Phone Number
    		<input class="myform__input" v-model="form.phone_number" id="phone_number" type="text">
		</label>

		<label class="myform__label">
			Do you live in the UK?<span class="myform__required">*</span><br/>
			<input class="myform__radio" v-model="form.live_in_uk" type="radio" name="live_in_uk" value=true> Yes<br>
			<input class="myform__radio" v-model="form.live_in_uk" type="radio" name="live_in_uk" value=false> No
		</label>

		<label class="myform__label" for="git_profile">Git Profile<span class="myform__required">*</span>
			<div class="myform__col-2">
				<div class="m-auto">https://github.com/</div>
				<input class="myform__input" v-model="clone__git_profile" id="git_profile" type="text">
			</div>
		</label>

		<label class="myform__label" for="firstName">
			CV<span class="myform__required">*</span>
			<Upload class="myform__input" @selectedFile="onSelectCV"/>
		</label>

		<label class="myform__label" for="firstName">
			Covering Letter
			<Upload class="myform__input" @selectedFile="onSelectCoverLetter"/>
		</label>

		<label class="myform__label" for="firstName">
			About You<span class="myform__required">*</span>
			<textarea class="myform__textarea" v-model="form.about_you">Tell me about yourselve..</textarea>
		</label>

		<!-- submit -->
		<button class="myform__submit" @click.stop.prevent="onSubmitForm()" >Submit</button>

	</form>
  </div>
</template>

<script>
import { URL, HEADER } from '~/config'
import axios from 'axios'

export default {
	data(){
		return {
			
			// error
			error: {},

			// local variables
			clone__git_profile: '',

			// form submit criteria
			form:{
				first_name: 'John',
				last_name: 'Doe', 
				email: 'johndoe@gmail.com',
				phone_number: '',
				live_in_uk: false,
				git_profile: 'https://github.com/johndoe',
				cv: null,
				// cover_letter: undefined,
				about_you: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
			}
		}
	},
	watch:{
		clone__git_profile(value){
			this.form.git_profile = 'https://github.com/' + value
		}
	},
	methods:{
		/**
		 * listen to selectedFile event from Upload child component
		 */
		onSelectCV(cv){
			console.warn("onSelectCV")
			this.form.cv = cv
			console.log(this.form.cv)
		},
		/**
		 * listen to selectedFile event from Upload child component
		 */
		onSelectCoverLetter(cl){
			console.warn("onSelectCoverLetter")
			this.form.cover_letter = cl
		},
		/**
		 * submit form
		 */
		onSubmitForm(){
			axios({
				method: 'post',
				url: URL,
				data: this.getFormData(),
				headers: {
					'X-Requested-With': 'XMLHttpRequest'},
			}).then((res) => {
				console.log(res)
			})
		},
		/**
		 * 
		 * @return { FormData } formData
		 */
		getFormData(){
			var formData = new FormData()
			Object.keys(this.form).forEach((name, i) => {
				formData.append(name, this.form[name])
			})
			return formData
		}
	}
}
</script>

<style>
.content {
	margin-left: 35vh;
	margin-right: 35vh;
	display: grid;
	grid-template-columns: 1fr;
}
/** steps */
#steps{
	@apply bg-gray-300 w-full mt-3;
	display: grid;
	grid-template-columns: 1fr 1fr 1fr;
}
.step{
	@apply justify-center flex text-gray-500;
}
.step-active{
	@apply bg-green-500 text-black;
}
.step > span{
	@apply m-auto m-1;
}
/** form */
.myform{
	@apply border px-8 pt-6 pb-8 rounded bg-white m-auto w-full bg-white shadow-md rounded;
	display: grid;
	grid-gap: 11px;
}
.myform__col-2{
	@apply font-normal;
	display:grid;
	grid-template-columns:0fr 3fr;
}
.myform__required{
	@apply text-red-600;
}
.myform__label{
	@apply block text-gray-700 font-bold;
}
.myform__input, .myform__textarea{
	@apply shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight m-1;
}
.myform__input:focus, .myform__textarea:focus{
	@apply outline-none shadow-outline;
}
.myform__textarea{
	@apply h-64;
}
.myform__radio{
	@apply m-1 mt-2;
}
.myform__submit{
	@apply bg-green-400 rounded py-2 text-white;
}
.myform__submit:hover{
	@apply bg-green-500;
}
</style>

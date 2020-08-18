<template>
	<div>
		<div class="component-wrapper">
			<h3 class="component-title">
				Create a signature
			</h3>
			<h4 class="component-subtitle">
				Use one of the following ways to create your signature
			</h4>
			<div class="tab-header">
				<button v-on:click="tab = 1" class="button-tab" v-bind:class="{ clicked: tab === 1 }" type="button">
					Type
				</button>
				<button v-on:click="tab = 2" class="button-tab" v-bind:class="{ clicked: tab === 2 }" type="button">
					Upload
				</button>
				<button v-on:click="tab = 3" class="button-tab" v-bind:class="{ clicked: tab === 3 }" type="button">
					Draw
				</button>
			</div>
			<div class="section-wrapper" v-show="tab === 1">
				<div>
					<input
						class="signature-input"
						type="input"
						id="input"
						ref="input"
						v-model="typedsignature"
						placeholder="Enter signature"
					/>
					<div class="button-wrappers">
						<button type="button" v-on:click="clearInput()">
							Clear
						</button>
						<button type="button" v-on:click="submitInput()">
							Submit
						</button>
					</div>
				</div>
			</div>
			<div class="section-wrapper" v-show="tab === 2">
				<div>
					<input type="file" id="file" ref="file" v-on:change="handleFileUpload()" />
					<button>
						Upload
					</button>
				</div>
			</div>
			<div class="section-wrapper" v-show="tab === 3">
				<div>
					<canvas class="signature"></canvas>
					<div class="button-wrappers">
						<button type="button" v-on:click="clearSignature()">
							Clear
						</button>
						<button type="button" v-on:click="submitSignature()">
							Submit
						</button>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import SignaturePad from "signature_pad";
export default {
	name: "SignaturePad",
	props: { value: String },
	data() {
		return {
			pad: null,
			/* Keeps track of the selected tab */
			tab: 1,

			file: "",

			typedsignature: "",
		};
	},

	mounted() {
		/* This will mount the canvas and make it usable */
		let canvas = document.querySelector("canvas");
		this.pad = new SignaturePad(canvas, {
			penColor: "rgb(230, 24, 24)",
			onEnd: () => {
				this.$emit("input", this.pad.toDataURL());
			},
		});
	},

	methods: {
		/* Clears the signature pad */
		clearSignature() {
			this.pad.clear();
			this.$emit("input", this.pad.toDataURL());
		},
		/* Clears the text input field */
		clearInput() {
			this.typedsignature = "";
		},
		handleFileUpload() {
			/*
                Select the first file
            */
			this.file = this.$refs.file.files[0];
		},

		submitFile() {
			/*
                Initialize the form data
            */
			let formData = new FormData();

			/*
                Add the form data we need to submit
            */
			formData.append("file", this.file);

			/* Send it to the backend like this:
            axios.post( '/api/signature',
                formData,
                {
                headers: {
                    'Content-Type': 'multipart/form-data'
                }
            })
            .then(function(){
                console.log("File successfully sent to backend");
            })
            .catch(function(err){
                console.error(err.code);
            });
        */
		},
		submitSignature() {
			/* Save the signature pad to an image */
			var signaturePadData = this.pad.toDataURL("image/jpeg");
			/*
                Initialize the form data
            */
			let formData = new FormData();

			/*
                Add the form data we need to submit
            */
			formData.append("file", signaturePadData);
			console.log(signaturePadData);

			/* Send it to the backend like this:
            axios.post( '/api/signature',
                formData,
                {
                headers: {
                    'Content-Type': 'multipart/form-data'
                }
            })
            .then(function(){
                console.log("File successfully sent to backend");
            })
            .catch(function(err){
                console.error(err.code);
            });
        */
		},
		submitInput() {
			console.log("Sending Typed Signature to Backend...");
			/* Send it to the backend like this:
            axios.post( '/api/signature',
                {
                    signature: this.typedsignature
                },
                {
                headers: {
                    'Content-Type': 'multipart/form-data'
                }
            })
            .then(function(){
                console.log("File successfully sent to backend");
            })
            .catch(function(err){
                console.error(err.code);
            });
        */
		},
	},
};
</script>

<style lang="scss">
$font-stack: Helvetica, sans-serif;

.button-tab {
	border: none;
	font-size: 1rem;
	background: #fff;
	padding: 0.5rem 1rem;
	cursor: pointer;
	box-sizing: border-box;
}
.clicked {
	border-bottom: 0.25rem solid rgb(0, 154, 230);
}
.signature-input {
	padding: 0.5rem;
	font-size: 1.5rem;
	border: none;
	margin-top: 6rem;
	border-bottom: 1px dotted rgb(131, 133, 136);
	width: 100%;
	box-sizing: border-box;
}
.button-wrappers {
	display: flex;
	justify-content: space-around;
	margin-top: 6rem;
}
.tab-header {
	display: flex;
	justify-content: center;
	border-bottom: thin solid rgb(0, 154, 230);
}
.component-title {
	color: rgb(0, 154, 230);
}
.component-subtitle {
	color: rgb(131, 133, 136);
}
.signature {
	border: 2px solid #cbc9c6;
	border-radius: 5px;
}
.component-wrapper {
	width: 32rem;
}
.section-wrapper {
	width: 100%;
	height: 100%;
	display: flex;
	align-items: center;
	justify-content: center;
	min-height: 16rem;
}
.section {
	width: 100%;
}
</style>

export default { }

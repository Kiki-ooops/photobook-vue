<template>
    <h3>Love u 3k</h3>
    <div>
        <input type='file' @input="onFileSelected" id="image_input" accept="image/png, image/jpg">
        <img src="" id="preview" width="200">
        <button @click="onUpload">Submit</button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            selectedFile: null,
            error: ""
        }
    },
  methods: {
    async onFileSelected(event) {
        this.selectedFile = event.target.files[0]
        console.log(this.selectedFile)
    },
    async onUpload(){
        const formData = new FormData()
        formData.append('file', this.selectedFile)

        //const image_input = document.getElementById("#image_input");
        const preview = document.getElementById("preview")

        const reader = new FileReader(); 
        reader.onload = function(e) {
            preview.setAttribute('src', e.target.result);
        }
        reader.readAsDataURL(this.selectedFile);

        const requestOptions = {
            method: "POST",
            body: formData
        }
        
        const file = await fetch("http://localhost:8080/file", requestOptions)
        .then(response => {
            if (response.status === 200) {
                console.log(response.body);
                return response.text();
            } else {
                console.log("Upload Failed");
            }
        })

        return file;
    }
  }
}
</script>
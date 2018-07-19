<template>
  <div>
       <ul>
        <li>Gradle download page: <a href="https://gradle.org/releases/" target="_blank">https://gradle.org/releases/</a></li>
        <li>DistributionUrl set in in gradle-wrapper.properties: <b>https\://services.gradle.org/distributions/gradle-<u>version</u>-all.zip</b></li>
        <li>Default download path: <b>~/.gradle/wrapper/dist/gradle-<u>version</u>-all/<font color='red'><u>folderName</u></font>/gradle-<u>version</u>-all.zip</b></li>
        <li>This page helps you get the folderName!</li>
      </ul>
      <input placeholder="Input gradle version" v-model="version" class="form_input">
      <h1> {{ folderName }} </h1>
  </div>
</template>

<script>
import md5 from 'js-md5'
import BigNumber from 'bignumber.js'
const PREFIX = "https://services.gradle.org/distributions/gradle-";
const SUFFIX = "-all.zip"
const ESCAPE_START = "https\\://"

export default {
  name: 'HelloWorld',
  data () {
    return {
      version: '',
    }
  },
  computed : {
    folderName:function(){
      let full = this.version.trim()
      if(full){
        if(full.indexOf("/") < 0){
          full = PREFIX + full + SUFFIX
        }else if(full.indexOf(ESCAPE_START) == 0){
          full = "https://" + full.substr(ESCAPE_START.length)
        }
        return new BigNumber(md5(full),16).toString(36).toLowerCase()
      }else{
        return ''
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
input {
  box-sizing: border-box;
  text-align:center;
  font-size:1.4em;
  height:2.7em;
  border-radius:4px;
  border:1px solid #c8cccf;
  -web-kit-appearance:none;
  -moz-appearance: none;
  display:block;
  outline:0;
  padding:0 1em;
  text-decoration:none;
  width:50%;
  display: inline-block;
  margin-top: 40px;
}
ul {
  text-align: left
}
</style>

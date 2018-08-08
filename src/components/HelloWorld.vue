<template>
  <div>
       <ul>
        <li>Gradle download page: <a href="https://gradle.org/releases/" target="_blank">https://gradle.org/releases/</a></li>
        <li>DistributionUrl set in in gradle-wrapper.properties: <b>https\://services.gradle.org/distributions/gradle-<u>version</u>-all.zip</b></li>
        <li>Default download path: <b>~/.gradle/wrapper/dist/gradle-<u>version</u>-all/<font color='red'><u>folderName</u></font>/gradle-<u>version</u>-all.zip</b></li>
        <li>Note:
          <ul>
            <li>
              <i>If your "DistributionUrl" start with "http" not "https",please input the "DistributionUrl" into the box below.</i>
            </li>
            <li>
              The radix is 32 in old version.(See the source code of gradder-wrapper.jar)
            </li>
            <li>
              Take a look at the <a href="https://github.com/wangqi060934/get-gradle-wrapper-folder-name/blob/master/README.md" target="_blank">Readme.md</a>.
            </li>
          </ul>
          </li>
        <li>This page helps you get the folderName!</li>
      </ul>
      <input placeholder="Input the gradle version or DistributionUrl" v-model="version" class="form_input">
      <select v-model="radix">
        <option value="36">Radix(default 36)</option>
        <option>32</option>
      </select>
      <h1> {{ folderName }} </h1>
  </div>
</template>

<script>
import md5 from 'js-md5'
import BigNumber from 'bignumber.js'
const PREFIX = "https://services.gradle.org/distributions/gradle-";
const SUFFIX_ALL = "-all.zip"
const ESCAPE_START_HTTPS = "https\\://"
const ESCAPE_START_HTTP = "http\\://"

export default {
  name: 'HelloWorld',
  data () {
    return {
      version: '',
      radix:36,
    }
  },
  computed : {
    folderName:function(){
      let full = this.version.trim()
      if(full){
        if(full.indexOf("/") < 0){
          if(full.indexOf("-all",full.length-"-all".length) > 0
            || full.indexOf("-bin",full.length-"-bin".length) > 0){
            full = PREFIX + full + ".zip"
          }else{
            full = PREFIX + full + SUFFIX_ALL
          }
        }else if(full.indexOf(ESCAPE_START_HTTP) == 0){
          full = "http://" + full.substr(ESCAPE_START_HTTP.length)
        }else if(full.indexOf(ESCAPE_START_HTTPS) == 0){
          full = "https://" + full.substr(ESCAPE_START_HTTPS.length)
        }
        return new BigNumber(md5(full),16).toString(parseInt(this.radix)).toLowerCase()
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
  outline:0;
  padding:0 1em;
  text-decoration:none;
  width:50%;
  display: inline-block;
  margin-top: 40px;
}
select {
  box-sizing: border-box;
  text-align:center;
  font-size:1.4em;
  height:2.7em;
  border-radius:4px;
  border:1px solid #c8cccf;
  -web-kit-appearance:none;
  -moz-appearance: none;
  outline:0;
  padding:0 1em;
  text-decoration:none;
  margin-top: 40px;
}
ul {
  text-align: left
}
</style>

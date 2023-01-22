<script>
import axios from 'axios';
import CodeMirror from './CodeMirror.svelte'
import { onMount } from 'svelte';
let defaultText = `function twoSum(arr, n) {
  for(let i = 0; i < arr.length; i++) {
      if (arr[i] + arr[j] === n) return true;
  }
  return false;
}
let name = 'Mark';

console.log('Hello ' + name + '!');
console.log('The answer is: ' + twoSum([1,3,9,5], 8));`

let textContent = defaultText;

let editor;
let result = {};

async function onClick() {
  const res = await axios.post(
      'https://syntax.textql.com' + '/fix',
      {
        code_js: textContent,
      },
  );
  console.log(res.data.code_js);
  result = res.data;
  
  editor.change(res.data.code_js);
  window.location.hash = btoa(res.data.code_js);
}

async function onRun() {
  const res = await axios.post(
      'https://syntax.textql.com' + '/run',
      {
        code_js: textContent,
      },
  );
  console.log(res.data.code_js);
  result = res.data;
  window.location.hash = btoa(textContent);
}

async function onChange() {
  window.location.hash = btoa(textContent);
}
async function onReset() {
  editor.change(defaultText);
}
</script>

<h1>Welcome to the Syntax-Fixer Demo</h1>
<p>Let's fix the code below!</p>

<CodeMirror on:change={onChange} bind:this={editor} bind:textContent={textContent}/>
<button on:click={onRun}>Run Code</button>
<button on:click={onClick}>Fix Errors!</button>
<button on:click={onReset}>Reset To Default</button>

<h2>Errors</h2>
{#if result.stderr}
  <pre>{result.stderr}</pre>
{/if}

<h2>Output</h2>
{#if result.stdout}
  <pre>{result.stdout}</pre>
{/if}

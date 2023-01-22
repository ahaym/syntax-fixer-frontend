<script>
import CodeMirror from './CodeMirror.svelte'
let textContent = `function twoSum(arr, n) {
  for(let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] + arr[j] === n) return true;
    }
  }
  return false;
}
let name = 'Mark';

console.log('Hello ' + name + '!');
console.log('The answer is: ' + twoSum([1,9,5], 8));`
</script>

<h1>Welcome to the Syntax-Fixer Demo</h1>
<p>Let's fix the code below!</p>

<CodeMirror bind:textContent={textContent}/>
<button>Fix Errors!</button>

An userscript to manipulate JS on Scorm System used by Vinh University.
# UserScript

This is the userscript source code.

```
// ==UserScript==
// @name Elearning Cheat Stuff
// @version 1.0
// @description An userscript to reveal correct answer in elearning
// @match        http://elearning.vinhuni.edu.vn/pluginfile.php/*
// @run-at document-start
// ==/UserScript==

let count = 0;
const oldsplit = window.String.prototype.split; // Grab original function
window.String.prototype.split = function(){ // Redefine the function
    if (typeof arguments.callee.caller.caller == 'function' && arguments.callee.caller.caller.toString().includes("reviewWithCorrectAnswers") && count == 0) {
        count = 1;
        let temp = arguments.callee.caller.caller.toString();
        const reg = /var c=[A-Za-z][A-Za-z]\(a\)\,d\=[A-Za-z][A-Za-z]\(c\.settings\(\)\)\;/
        temp = temp.replace(reg, 'return "reviewWithCorrectAnswers";');
        let newfunction = new Function(temp.substring(temp.indexOf('{')+1,temp.lastIndexOf('}')));
        newfunction();
        return;
    }
    return oldsplit.apply(this, arguments); // Use .apply to continue as normal
}
```

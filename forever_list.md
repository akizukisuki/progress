info:    Forever stopped process:
    uid  command       script  forever pid  id logfile                    uptime       
[0] 1ZVb /usr/bin/node test.js 2377    2383    /home/pi/.forever/1ZVb.log 0:0:2:46.939 


https://blog.csdn.net/llzkkk12/article/details/78171750
https://segmentfault.com/q/1010000008719439




const exec = require('child_process').exec

const build = exec('sudo ntpdate time.google.com')
var sec=1000;
setInterval(function()
{
	build.stdout.on('data', data => console.log('stdout: ', data))
},sec);

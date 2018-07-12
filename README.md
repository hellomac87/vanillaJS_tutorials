# vanillaJS_tutorials

# AJAX (Asynchronous JavaScript and XML)
- ajax 통신하기 예제
- XMLHttpRequest, JQuery ajax, fetch api를 사용하여 https://yts.am 에서 제공하는 api 데이터를 출력해본다.
- 사용할 api url(https://yts.am/api/v2/list_movies.json?sort_by=download_count&limit=50)


# basic source

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        #load{position:fixed;top:0;left:0;right:0;bottom:0;color:#ffffff;background: rgba(0,0,0,0.5);display: none;justify-content: center;align-content: center;align-items: center;}
    </style>
</head>
<body>
    <div id="load">loading</div>
    <input type="button" id="btn" value="call api">
    <div id="root"></div>
</body>
</html>
```
- call api 버튼 누르면 root 엘리먼트에 출력되도록
- 불러오는 도중에 progress 화면 생성, 완료시 사라지게

# XMLHttpRequest  
```javascript
const rootElem = document.getElementById('root');
const btn = document.getElementById('btn');
btn.addEventListener('click', callApi);

function callApi(){
    // console.log('call API');

    var req = new XMLHttpRequest();
    req.open('GET', 'https://yts.am/api/v2/list_movies.json?sort_by=download_count&limit=50', true);
    req.onprogress = function(){
        console.log('on progress');
        document.getElementById('load').style.display = 'flex';
    }
    req.onload = function(){
        let res = JSON.parse(this.responseText);

        // console.log(res.data.movies);

        let movies = res.data.movies;

        movies.map(function(movie){

            let listItem = document.createElement('div');

            listItem.innerHTML += '<div>' +'<img src="'+ movie.medium_cover_image + '"/>'+ '</div>';
            listItem.innerHTML += '<div>' + movie.title + '</div>';
            listItem.innerHTML += '<div class="ratings">' + movie.rating + '</div>';

            rootElem.appendChild(listItem);
        });

        document.getElementById('load').style.display = 'none';
    }
    req.send(null);
}
```

# JQUERY AJAX
```javascript
const $rootElem = $('#root');
const $btn = $('#btn');
$btn.on('click', callApi);

function callApi(){
    //console.log('call API');

    $.ajax({
        url:'https://yts.am/api/v2/list_movies.json?sort_by=download_count&limit=50',
        beforeSend:function(){
            console.log('before');
            $('#load').css('display','flex');
        },
        complete:function(){
            console.log('complete');
            $('#load').hide();
        }
    })
    .done((result) =>{
        // console.log(result.data.movies);
        let movies = result.data.movies;
        
        movies.map(function(movie){
            // console.log(movie);

            let listItem = document.createElement('div');

            listItem.innerHTML += '<div>' +'<img src="'+ movie.medium_cover_image + '"/>'+ '</div>';
            listItem.innerHTML += '<div>' + movie.title + '</div>';
            listItem.innerHTML += '<div class="ratings">' + movie.rating + '</div>';

            $rootElem.append(listItem);
        });
    })
    .fail(err =>
        console.log(err)
    );
}
```

# fetch api
```javascript
const rootElem = document.getElementById('root');
const btn = document.getElementById('btn');
btn.addEventListener('click', callApi);

function callApi(){
    //console.log('call API');

    fetch('https://yts.am/api/v2/list_movies.json?sort_by=download_count&limit=50')
    .then((response) =>{
        return response.json();
    })
    .then((result) => {
        // console.log(result.data.movies);
        let movies = result.data.movies;
        
        movies.map(function(movie){
            // console.log(movie);

            let listItem = document.createElement('div');

            listItem.innerHTML += '<div>' +'<img src="'+ movie.medium_cover_image + '"/>'+ '</div>';
            listItem.innerHTML += '<div>' + movie.title + '</div>';
            listItem.innerHTML += '<div class="ratings">' + movie.rating + '</div>';

            rootElem.appendChild(listItem);
        });
    })
    .catch(err =>
        console.log(err)
    );
}
```
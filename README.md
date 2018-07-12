# vanillaJS_tutorials

# AJAX (Asynchronous JavaScript and XML)
- ajax 통신하기 예제
- ajax가 대충 뭔지는 알겠는데 실제로 어떻게 써야하는지 막막해서 내가그냥 해보는 예제
- XMLHttpRequest, JQuery ajax, fetch api를 사용하여 https://yts.am (뭔 영화 토렌트 사이트라고함)에서 제공하는 api 데이터를 출력해본다.
- 사용할 api url(https://yts.am/api/v2/list_movies.json?sort_by=download_count&limit=50)


# Basic html source

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
- 버튼을 누른다 => 로딩바가 뜬다 => 영화목록이 출력된다.(로딩바사라짐) => 쨔잔


# fetch api
- 처음엔 그냥 fetch api한번 사용해보려고 했음
- 예전에 리액트 튜토리얼 보면서 따라할때 간단하게 fetch를 쓰면 불러올수 있어서 그냥 되나 안되나 해봄 
- 근데 문제가 생겼음, 불러올때 로딩바같은거 progress 라고 하나 그걸 띄워보고 싶은데 fetch api는 그걸 제공해주지 않는다고함
- 그래서 jquery ajax랑 xmlhttprequest를 써보기로 함


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


# JQUERY AJAX
- 제일 접근하기 쉬운 제이쿼리로 해봄
- 여기서는 beforeSend, complete 메소드를 제공해줘서 그냥 때려박으면 댐

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

## ajax 참고링크
### fetch api
- https://blog.perfectacle.com/2017/01/25/es6-ajax-with-fetch/
- https://medium.com/@kkak10/javascript-fetch-api-e26bfeaad9b6
- https://www.zerocho.com/category/HTML/post/595b4bc97cafe885540c0c1c
- https://developer.mozilla.org/ko/docs/Web/API/Fetch_API/Fetch%EC%9D%98_%EC%82%AC%EC%9A%A9%EB%B2%95

### $.ajax api
- http://api.jquery.com/jquery.ajax/
-

### XmlHttpRequest api
-
-




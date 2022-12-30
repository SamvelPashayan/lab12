<p <b> align = "center">МИНИСТЕРСТВО НАУКИ И ВЫСШЕГО ОБРАЗОВАНИЯ
РОССИЙСКОЙ ФЕДЕРАЦИИ
ФЕДЕРАЛЬНОЕ ГОСУДАРСТВЕННОЕ БЮДЖЕТНОЕ
ОБРАЗОВАТЕЛЬНОЕ УЧРЕЖДЕНИЕ ВЫСШЕГО ОБРАЗОВАНИЯ
«САХАЛИНСКИЙ ГОСУДАРСТВЕННЫЙ УНИВЕРСИТЕТ» </b> </p>
<br>
<p align = "center">Институт естественных наук и техносферной безопасности</p>
<p align = "center">Кафедра информатики</p>
<p align = "center">Пашаян Самвел Алексанович</p>
<br>
<p align = "center">Лабораторная работа №12 NodeJS</p>
<p align = "center">01.03.02 Прикладная математика и информатика</p>
<br>
<p align = "right" >Научный руководитель</p>
<p align = "right" >Соболев Евгений Игоревич</p>
<p align = "center" >Южно-Сахалинск</p>
<p align = "center" >2022 г.</p>
<p align = "center" ><b>ВВЕДЕНИЕ</b></p>
<p> <b> JavaScript </b> — это язык программирования, который используют для написания frontend- и backend-частей сайтов, а также мобильных приложений. Часто в текстах и обучающих материалах название языка сокращают до JS. Это язык программирования высокого уровня, то есть код на нем понятный и хорошо читается.</p>
<p> JavaScript обычно используется как встраиваемый язык для программного доступа к объектам приложений. Наиболее широкое применение находит в браузерах как язык сценариев для придания интерактивности веб-страницам, для этого даже не требуется компиляция (перевод языка программирования в машинный код). Скрипты можно прописать внутри кода страницы или подключить к HTML отдельным файлом. 
Основные архитектурные черты: динамическая типизация, слабая типизация, автоматическое управление памятью, прототипное программирование, функции как объекты первого класса.</p>
<p align = "center" > РЕШЕНИЕ ЗАДАЧ (ОСНОВНАЯ ЧАСТЬ) </p>

- - -


## Решение:
### APP.JS
    const fs = require('fs');

    const dirs=fs.readdirSync("./gallery", { withFileTypes: true })
    .filter(d => d.isDirectory())
    .map(d => d.name);

    fs.writeFile('./jsons/allalbums.json',JSON.stringify(dirs),'utf8',function(err){
        console.log(err);
    });

    for (let i = 0; i < dirs.length; i++) {
        const images=fs.readdirSync('./gallery/'+dirs[i]);
        fs.writeFile('./jsons/'+dirs[i]+'.json',JSON.stringify(images),'utf8',function(err){
            console.log(err);
        });
        
    }

    const express = require("express");
    const app = express();
    app.use(express.static(__dirname ));
    app.listen(3000, function(){
        console.log("Сервер ожидает подключения...");
    });
### index.html
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width" />
        <title>Галерея</title>
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-Zenh87qX5JnK2Jl0vWa8Ck2rdkQ2Bzep5IDxbcnCeuOxjzrPF/et3URy9Bv1WTRi" crossorigin="anonymous">
    </head>
    <style>
        #gallery {
            width: 100%;
            display: grid;
            grid-template-columns: repeat(3, 30%);
            grid-gap: 1%;
            margin: 0 auto;
            justify-content: center;
            margin-top: 2%;
            margin-bottom: 2%;
            row-gap: 35px;
        }
        img {
        width: 100%;
        height: 100%;
        background: #341b9b;
        }
        img:hover {opacity: 0.7;}
    </style>
    <body style="background-color: #0c0e1d;">
        <div  class="text-warning p-3 bg-blue" >
        <h1 style="text-align: center;">Галерея</h1>
        <select id="selectNumber" style="background-color: #e2e2e2; margin-left: 10%; width: 80%; text-align: center;" class="form-select" >
            <option selected>Выберите Альбом</option>
        </select>
        </div>
        <div id="gallery">
    </div>

        <script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>
        <script>
            //считываем json файл с альбомами
            //fetch('./jsons/allalbums.json')
            fetch('https://jsonplaceholder.typicode.com/albums')
            .then((response) => {
                return response.json();
            })
            .then((data) => {
                //добавление альбомов в выпадающий список
                var select = document.getElementById("selectNumber"); 
                for(var i = 0; i < data.length; i++) {
                    var opt = data[i].title;
                    var el = document.createElement("option");
                    el.textContent = data[i].title;
                    el.value = data[i].id;
                    select.appendChild(el);
                    
                }
            });
            //действие при выборе селекта
            $('#selectNumber').on('input', function() {
            let currentalbum= $('#selectNumber').val();
                $("#gallery").html("");
            showImage(currentalbum);
                
            });
            //вывод картинок
            function showImage(album){
                let gallery=document.getElementById("gallery");
                //fetch('./jsons/'+album+'.json')
                fetch('https://jsonplaceholder.typicode.com/photos?albumId='+album)
                .then((response) => {
                    return response.json();
                })
                .then((data) => {
                    for (let i = 0; i < data.length; i++) {
                        const image=document.createElement("img");
                        const a=document.createElement('a');
                        //a.href='./gallery/'+album+'/'+data[i];
                        a.href=data[i].url;
                        //image.src='./gallery/'+$('#selectNumber').val()+'/'+data[i];
                        image.src=data[i].thumbnailUrl;
                        image.className="rounded";
                        a.append(image);
                        gallery.append(a);
                    }
                });
            }
            
        </script>
</body>
</html>

- - -

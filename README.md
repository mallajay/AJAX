# Simple-AJAX-CRUD-Using-JavaScript

# 1. Install
    1. install node and npm
    2. to create JSON Server `Install npm i json-server -g`
    3. `npm init`
    4. `npm install json-server --save-dev`

# 2. Image Refernece 
![](Image/image.PNG)

# 3. AJAX Example Using XHR
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>API DEMO</title>
  </head>

  <body>
    <div class="output1"></div>
    <div class="output2"></div>
    <script>
      const output1 = document.querySelector(".output1");
      const output2 = document.querySelector(".output2");

      var xhr = new XMLHttpRequest();
      xhr.onreadystatechange = function() {
        if (xhr.readyState === 4 && xhr.status === 200) {
          let temp = JSON.parse(xhr.responseText);
          console.log(temp);

          for (let x = 0; x < temp.results.length; x++) {
            console.log(temp.results[x]);
            let person = temp.results[x];
            output1.innerHTML += `${person.name.first} ${person.name.last}
            <img src="${person.picture.thumbnail}"> <br>`;
          }
        }

        //XHR onLoad Method
        xhr.onload = function() {
          console.log("done");
        };

        //XHR onProgress Method
        xhr.onprogress = function() {
          console.log("loading");
        };
      };
      xhr.open("get", "https://randomuser.me/api/?results=10");
      xhr.send();
    </script>
  </body>
</html>
```
# 4. AJAX Example Using fetch
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>API DEMO</title>
  </head>

  <body>
    <div class="output1"></div>
    <div class="output2"></div>
    <script>
      const output1 = document.querySelector(".output1");
      const output2 = document.querySelector(".output2");

      const url = "https://randomuser.me/api/?results=10";
      fetchData();

      function fetchData() {
        fetch(url)
          .then(res => {
            return res.json();
          })
          .then(data => {
            console.log(data.results);
            let people = data.results.map(function(el) {
              console.log(el);
              return el.name.first + " " + el.name.last;
              // return el;
            });
            console.log("People", people);

            people.forEach(person => {
              console.log(person);
              output1.innerHTML += `${person}<br>`;
            });
          });
      }
    </script>
  </body>
</html>
```
# 5. AJAX In TypeScript 
```
import { Component, OnInit } from "@angular/core";

@Component({
  selector: "app-list",
  templateUrl: "list.page.html",
  styleUrls: ["list.page.scss"]
})
export class ListPage implements OnInit {

  productList: Array<any> = [];
  url = "https://randomuser.me/api/?results=10";

  constructor() {
    this.fetchData();
  }

  ngOnInit() {}

  fetchData() {
    let result = fetch(this.url)
      .then(res => {
        return res.json();
      })
      .then(data => {
        console.log(data.results);
        let people = data.results.map(function(el) {
          // console.log("El", el);
          return el;
        });
        //Very Imp In TypeScript for front End
        this.productList = people;
        console.log("People", people);
      });
    }
}
```
### In .html File
```
<ion-content>
  <ion-grid>
    <ion-row *ngFor="let pro of productList">
      <ion-col>
        <h6>
          {{pro.name.first}} {{pro.name.last}}
          <ion-img
            style="height: 50px; width: 50px;"
            src="{{ pro.picture.thumbnail }}"
          ></ion-img>
        </h6>
        <br />
      </ion-col>
    </ion-row>
  </ion-grid>
</ion-content>
```

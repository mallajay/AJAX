# Simple-AJAX-CRUD-Using-JavaScript
##### Note: Clone and Run 
    1. AJAX Example Using XHR and 
    2. AJAX Example Using fetch
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
import { WidgetUtilService } from './../providers/widget-util.service';

@Component({
  selector: "app-list",
  templateUrl: "list.page.html",
  styleUrls: ["list.page.scss"]
})
export class ListPage implements OnInit {

  productList: Array<any> = [];
  productListAvailable: boolean = false;

  constructor() {
    this.fetchData();
  }

  ngOnInit() {}

   getProductList() {
    try {
      this.productListAvailable = false;
      let result = fetch(
        'URL'
      )
        .then((res) => {
          return res.json();
        })
        .then((data) => {
          // console.log(' Product data ', data);
          let product = data.map((el) => {
            return el;
          });
          //Very Imp In TypeScript for front End
          this.productList = product;
          console.log('product', product);
          this.productListAvailable = true;
        });
    } catch (error) {
      console.log(error);
      this.widgetUtilService.showToast(error.message, 'ERROR');
      this.productListAvailable = true;
    }
  }
}
```

# 6. Fetch Request with ID token in Authorization header

```
import { WidgetUtilService } from './../providers/widget-util.service';
import { Component, OnInit } from '@angular/core';
import { AngularFireAuth } from '@angular/fire/auth';

@Component({
  selector: 'app-home',
  templateUrl: 'home.page.html',
  styleUrls: ['home.page.scss'],
})
export class HomePage implements OnInit {
  productList: Array<any> = [];
  productListAvailable: boolean = false;

  constructor(
    private widgetUtilService: WidgetUtilService,
    private angularFireAuth: AngularFireAuth
  ) {
    this.onAuthStateChanged();
  }

  ngOnInit() {}

  onAuthStateChanged() {
    this.angularFireAuth.auth.onAuthStateChanged((user) => {
      if (user) {
        this.authenticatedProductDataByUsingFETCHRequest();
      }
    });
  }
  
  
   // By Using Fetch Method Auth data
   

   
  authenticatedProductDataByUsingFETCHRequest() {
    try {
      this.productListAvailable = false;
      let result = this.angularFireAuth.auth.currentUser.getIdToken().then(
        function(token) {
          console.log('Sending request with ID token in Authorization header.');
          fetch(
            'URL',
            {
              method: 'GET',
              headers: new Headers({
                Authorization: 'Bearer ' + token,
              }),
            }
          )
            .then((res) => {
              return res.json();
            })
            .then((data) => {
              // console.log(' Product data ', data);
              let product = data.map((el) => {
                return el;
              });
              //Very Imp In TypeScript for front End
              this.productList = product;
              console.log('Data product', product);
              this.productListAvailable = true;
            });
        }.bind(this)
      );
    } catch (error) {
      console.log(error);
      this.widgetUtilService.showToast(error.message, 'ERROR');
      this.productListAvailable = true;
    }
  }
}
```

# 7. XMLHttpRequest with ID token in Authorization header

```

import { WidgetUtilService } from './../providers/widget-util.service';
import { Component, OnInit } from '@angular/core';
import { AngularFireAuth } from '@angular/fire/auth';

@Component({
  selector: 'app-home',
  templateUrl: 'home.page.html',
  styleUrls: ['home.page.scss'],
})
export class HomePage implements OnInit {
  productList: Array<any> = [];
  productListAvailable: boolean = false;

  constructor(
    private widgetUtilService: WidgetUtilService,
    private angularFireAuth: AngularFireAuth
  ) {
    this.onAuthStateChanged();
  }

  ngOnInit() {}


  onAuthStateChanged() {
    this.angularFireAuth.auth.onAuthStateChanged((user) => {
      if (user) {
        this.authenticatedProductDataByUsingFETCHRequest();
        this.authenticatedProductDataByUsingXMLHttpRequest();
      }
    });
  }

  
   // GET product List With Auth Check by Using XMLHttpRequest
 

  authenticatedProductDataByUsingXMLHttpRequest() {
    try {
      this.productListAvailable = false;
      let result = this.angularFireAuth.auth.currentUser.getIdToken().then(
        function(token) {
          console.log('Sending request with ID token in Authorization header.');

          var xhr = new XMLHttpRequest();

          xhr.onreadystatechange = function() {
            // console.log('XHR', xhr);

            if (xhr.readyState === 4 && xhr.status === 200) {
              // console.log(xhr.responseText);
              let temp = JSON.parse(xhr.responseText);
              console.log('XMLHttpRequest: Product Data', temp);
              let productList = temp;
            }

            //XHR onLoad Method*
            xhr.onload = function() {
              console.log('done');
            };

            //XHR onprogress Method*
            xhr.onprogress = function() {
              console.log('loading');
            };
          };

          //XHR Open Method*
          xhr.open(
            'GET',
            'URL',
            true
          );
          xhr.setRequestHeader('Authorization', 'Bearer ' + token);
          xhr.send();

          this.productListAvailable = true;
        }.bind(this)
      );
    } catch (error) {
      console.log(error);
      this.widgetUtilService.showToast(error.message, 'ERROR');
      this.productListAvailable = true;
    }
  }

 
}

```

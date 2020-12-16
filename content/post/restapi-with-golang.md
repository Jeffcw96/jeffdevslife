+++
author = "Jeff Chang"
title = "REST API with Go"
date = "2020-12-15"
description = "In this article, we are going to build a simple restful service with Golang by using Gorilla Mux package"
tags = [
    "golang", 
]
metakeywords = "golang, restful, rest api, restful service"
+++

In this article, we are going to build a simple restful service with Golang by using Gorilla Mux package.

### Prerequisites:

* Before everything started. Please make sure you have installed [GO and set as your environment variable.](https://golang.org/doc/install).
* It would be better if you already understand how to [create a simple web server with GO](/post/setup-go-server).

We will be covered 4 common methods in our REST API as well as the testing tool for testing each endpoints
* [Postman](#postman)
* [POST](#post)
* [GET](#get)
* [PUT](#put)
* [DELETE](#delete)

## Postman<a name="postman"></a>
[Postman](https://www.postman.com/downloads/) is a great tool when trying to dissect RESTful APIs made by others or test ones you have made yourself. Their interface is very user-friendly. For example we can always specific different HTTP methods *like POST, GET, PUT, and etc* from the dropdown menu. 

Figure below is showing a simple way to create a request as well as categorize or group them into a collection
![Postman user interface](/images/go-rest-1.png)

We are making a sample POST request. *Noted: we can define our JSON body by clicking **Body** > **raw***
![Postman Post Request](/images/go-rest-2.png)

Before we started to make any HTTP requests. There are few packages we going to use in this example:
{{< highlight go >}}
import (
	"encoding/json"
	"fmt"
	"math/rand"
	"net/http"
	"strconv"

        //package to serve our HTTP request and server
	"github.com/gorilla/mux"
)
{{< /highlight >}}

We also need to declare a struct and a slice so that we can use and append them later on in the memory.
{{< highlight go >}}
type User struct {
	Id         string `json:"id"`
	Name       string `json:"name"`
	Age        int64  `json:"age"`
	Occupation string `json:"occupation"`
}

var allUsers []User
{{< /highlight >}}

#### Setup Server and registered routes
{{< highlight go >}}
func main() {
	router := mux.NewRouter()

        //router.HandleFunc("<endpoint>", <function name>).Methods("<HTTP method>")
	router.HandleFunc("/createUser", CreateUser).Methods("POST")
	router.HandleFunc("/getAllUsers", GetUsers).Methods("GET")
	router.HandleFunc("/getUser/{id}", GetUserById).Methods("GET")
	router.HandleFunc("/updateUser/{id}", UpdateUser).Methods("PUT")
	router.HandleFunc("/deleteUser/{id}", DeleteUser).Methods("DELETE")

	fmt.Println("Server is at Port 3000")
	http.ListenAndServe(":3000", router)
}
{{< /highlight >}}
As you can see there are multiple routes function above which will be the examples we going to discussed later on
Now, run the file by inserting the commands in command prompt `go build` , `go run main.go`. Then you should see the sentence <br/>
`Server is at Port 3000` in the log.

## POST<a name="post"></a>
Let's start with our very first POST request. As showing from the code above. We are using the function `CreateUser` to serve the router url `/createUser`. <br/> 
And this is how the function looks like:

{{< highlight go >}}
func CreateUser(w http.ResponseWriter, r *http.Request) {
	var user User                              //initialize with struct
	json.NewDecoder(r.Body).Decode(&user)      //passing JSON object to struct
	user.Id = strconv.Itoa(rand.Intn(1000000)) //Generate random ID
	allUsers = append(allUsers, user)          //save user into all users in memory
	w.Header().Set("Content-Type", "application/json; charset=UTF-8")
	w.WriteHeader(http.StatusCreated)
	json.NewEncoder(w).Encode("user Created")
}
{{< /highlight >}}
##### Explanation
1. Initialize the variable with struct (User)
2. Decode the JSON object from request body into the variable
3. Assign ID to the object. <small><em>Note: Assign random number as ID is not recommended as it has the duplicated possibility</em></small>
4. Save the variable into array as total user


##### Test POST Request <small><code>http://localhost:3000/createUser</code></small>
Let's test this API by calling it from our POSTMAN
![Create User Post Request](/images/go-rest-3.png)

## GET<a name="get"></a>
This API is designed to get all user information which basically return the value from the array `allUsers` <br/>
Please call the `CreateUser` API for few more times so that we can observe the result clearly.

Let's see how the function (`GetUsers`,  route url = `/getAllUsers`) looks like:
{{< highlight go >}}
func GetUsers(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "application/json; charset=UTF-8")
	json.NewEncoder(w).Encode(allUsers)
}
{{< /highlight >}}
##### Explanation
1. You may wondering why we can directly encode and response back with the array variable `allUsers`
2. It's because currently, all data are being stored in the memory. In the sense that, all data from the variable will be gone if we restart our server.
3. Is real world application. Those data most likely are being stored in the database. For example: 
    *  Every time user make API request, we first capture and have some internal process such as filtering, conditional matching and so on.
    *  Then we will store them are get them from the database

##### Test GET Request <small><code>http://localhost:3000/getAllUsers</code></small>
![Get All Users via Get Request](/images/go-rest-4.png)
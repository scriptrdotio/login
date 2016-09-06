# login 
(Experimental)


##Components
- /modules/login/widgets/login.js  widget that is can be place in an HTML file to provide login for devices and users.
- /modules/login/widgets/authorization.js widget that can authenticate the token within the cookie, and invoke callbacks based on whether or not the token is valid or invalid. it'll validate the token when the widget is created and every 10 seconds.
- /modules/login/html/login.html  Prexisting template that allows you to have the login form in your project with minimum effort. you can take it as is or modify it.
- /modules/login/html/home.html A sample home page that demonstrates the use of the authorization.js widget
- /modules/login/css/style.css  a simple css file that will allow you control the styling of the login form applied on the /modules/login/test/login.html
- /modules/login/config configuration file for the login server side script.
- /modules/login/login  server side scriptr script that logins the user device , returns a bearer token and puts it in a cookie.

##Usage 
###Login Widget 
####Creating the login widget
```  
 $('<jQuerySelectorToForm>').loginWidget({redirectTarget:"<relative Path To Home Page>"});
```  
Note that you must have form that looks like the form in the following example.

#### Example

```
<!DOCTYPE html>
<html>  
	<head> 
		<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
		<title>Log In | scriptr.io</title>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <link rel="shortcut icon" type="image/x-icon" href="https://www.scriptr.io/themes/scriptr/images/favicon.ico">
		<link rel="shortcut icon" href="https://www.scriptr.io/themes/scriptr/images/favicon.ico" type="image/png" />
      	<script src="https://code.jquery.com/jquery-1.9.1.js" ></script>
		<script src="https://code.jquery.com/ui/1.9.1/jquery-ui.js"></script>
		<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" ></script>
      	<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery.bootstrapvalidator/0.5.3/js/bootstrapValidator.min.js" ></script>
                 <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-cookie/1.4.1/jquery.cookie.min.js" ></script>
      	<script src="../widgets/login.js"> </script>
        <script src="../widgets/authorization.js"> </script>
		<link href='https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css' rel='stylesheet' type ='text/css' />
      	<link href='https://cdnjs.cloudflare.com/ajax/libs/jquery.bootstrapvalidator/0.5.3/css/bootstrapValidator.min.css' rel='stylesheet' type ='text/css' />
  		<link href='//fonts.googleapis.com/css?family=Raleway:300,800' rel='stylesheet' type='text/css'>
		<link href='//fonts.googleapis.com/css?family=Source+Sans+Pro:300,400,600,700' rel='stylesheet' type='text/css'>
		<link href='//fonts.googleapis.com/css?family=Roboto:400,500,700,300' rel='stylesheet' type='text/css'>
		<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.6.1/css/font-awesome.min.css">
		<link rel="stylesheet" href="../css/style.css">
	</head>
	<body>
      
      <!-- begin header with company logo -->
      <header class="text-center">
      	<img src="https://blog.scriptr.io/wp-content/uploads/2014/11/logo.png" />
      </header>
  
      <!-- begin page title -->    
      <h2 class="text-center lead-title"><b>Login</b></h2>
      <div class="header-strips-one"></div>
          
      <!-- begin form -->      
      <div class="col-xs-12 col-sm-6 col-md-4 form-wrap">
        <form id="loginForm">
          <h7 id="errorMessage" class="error"> </h7>
          <div class="form-group">
            <label for="deviceId"> ID</label>
            <input class="form-control" id="id" name="id" placeholder="ID">
          </div>
          <div class="form-group">
            <label for="password">Password</label>
            <input type="password" class="form-control" name="password" id="password" placeholder="Password">
          </div>
          <button  type="button" class="btn btn-default btn-block text-uppercase">Login</button>
        </form>
      </div>
      <!-- end form  -->
    
      
      <script type="text/javascript">
		//here we set the login form to be the form with id loginForm . Upon login, the user will be redirected to ./home.html
		// the form must an input with the id id or user/device id, and an input with the id password for the password.
	
        $('#loginForm').loginWidget({redirectTarget:"./home.html"});
      </script>
	</body>
</html>
```

###Authorization Widget 
####Creating the authorization widget 

```
 var authorization  = $.scriptr.authorization({onTokenValid:function(){
           		///define what happens if the token is valid
            var token = this.getToken();

			
          }, loginPage:"<relative path to login page if onTokenInvalid is ommitted>" , onTokenInvalid:function(){
				//override the default behavior if the token is invalid
			}  });
```

```
<!DOCTYPE html>
<html>  
	<head> 
		<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
		<title>Home Page</title>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <link rel="shortcut icon" type="image/x-icon" href="https://www.scriptr.io/themes/scriptr/images/favicon.ico">
		<link rel="shortcut icon" href="https://www.scriptr.io/themes/scriptr/images/favicon.ico" type="image/png" />
      	<script src="https://code.jquery.com/jquery-1.9.1.js" ></script>
		<script src="https://code.jquery.com/ui/1.9.1/jquery-ui.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-cookie/1.4.1/jquery.cookie.min.js" ></script>
    	<script src="../widgets/authorization.js" ></script>
      	<link href='//fonts.googleapis.com/css?family=Raleway:300,800' rel='stylesheet' type='text/css'>
		<link href='//fonts.googleapis.com/css?family=Source+Sans+Pro:300,400,600,700' rel='stylesheet' type='text/css'>
		<link href='//fonts.googleapis.com/css?family=Roboto:400,500,700,300' rel='stylesheet' type='text/css'>
		<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.6.1/css/font-awesome.min.css">
      
      	<script type="text/javascript">
        // you can create the authorization widget, upon creation, the widget will authenticate the bearer token within the cookie         	                		//set by the login widget, if the cookie is not valid, it'll redirect you to the file specified to loginPage. 	
		// you can override the default invalid token callback behavior by passing the onTokenInvalid as a function that does 	                                             //whatever you want.

          var authorization  = $.scriptr.authorization({onTokenValid:function(){
            	$(".container").find("h1").text("HELLO " + this.user.name);
            
          }, loginPage:"./login.html"});
         </script>
      
	</head>
	<body>
      <div class="container">
        <h1>HELLO  </h1>
     
      </div>
		
	</body>
</html> 
``` 





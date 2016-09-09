# About the login Module
This is an experimental module that provides out of the box login/logout functionality on the server side as well as the client side. The client side consists of a widget that can be included in any HTML file and easily styled. The server side consists of an authorization component for allowing logging in and out.

##Components
- /modules/login/widgets/login.js: a widget that can be placed in an HTML file to provide login for devices and users.
- /modules/login/widgets/authorization.js: a widget that can authenticate the token within the cookie, and invoke callbacks based on whether or not the token is valid. It will validate the token when the widget is created, then every 10 seconds thereafter.
- /modules/login/html/login.html: a template that allows you to have the login form in your project with minimum effort. You can use it "as is" or customize it as you see fit.
- /modules/login/html/home.html: a sample home page that demonstrates the use of the authorization.js widget.
- /modules/login/css/style.css: a simple CSS file that will allow you to control the styling of the login form. It is applied to the /modules/login/test/login.html file.
- /modules/login/login: a server side scriptr script that logs the user device in, returns a bearer token, then saves it in a cookie.

##Usage
###Login Widget 
####Creating the login widget

The login widget should be used with the scriptr.io subdomain configured on your account. ( Go to Account - > SUB-DOMAIN)

```  
 $('<jQuerySelectorToForm>').loginWidget({redirectTarget:"<relative Path To Home Page>", anonymousToken:"<Your scriptr.io anonymous token>" ,expiry: <number of hours>});
 
 //expiry is the number of hours before the session is invalidated.
 
```  
Note that you must have a form that looks like the form in the following example.

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
      	<script src="https://code.jquery.com/jquery-1.9.1.min.js" ></script>
		<script src="https://code.jquery.com/ui/1.9.1/jquery-ui.min.js"></script>
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
	      <h1 class="text-center lead-title"><b>Login</b></h1>
	      <div class="header-strips-one"></div>
	          
	      <!-- begin form -->      
	      <div class="container">
	        <div class="col-sm-5 col-md-4 col-xs-9 form-wrap">
	          <form id="loginForm">
	              <div id="errorMessage" class="error alert alert-danger hide"></div>
	              <div class="form-group">
	                <input class="form-control" id="id" name="id" placeholder="Username">
	              </div>
	              <div class="form-group">
	                <input type="password" class="form-control" name="password" id="password" placeholder="Password">
	              </div>
	         	   <button id="submitBtn"  class="btn btn-default btn-block text-uppercase" >Login</button>
				  <div id="loadingDiv" class="text-center" style="display:none"> <i class="fa fa-spinner fa-spin fa-2x fa-fws"></i></div>
	          </form>
	        </div>
	      </div>
	      <!-- end form  -->
	    
	      
	      <script type="text/javascript">
	        $('#loginForm').loginWidget({redirectTarget:"./home.html"});
	      </script>
	</body>
</html>

```

###Authorization Widget 
####Creating the authorization widget 

As soon as the authorization widget is created, it will validate the token then keep validating the token every 10 seconds. The default behavior if the token is invalid is being redirected to the loginPage property. But that behavior can be overriden by passing the onTokenInvalid function.

```
 var authorization  = $.scriptr.authorization({onTokenValid:function(){
           		///define what happens if the token is valid
            var token = this.getToken();

			
          }, loginPage:"<relative path to login page if onTokenInvalid is ommitted>" , onTokenInvalid:function(){
				//override the default behavior if the token is invalid
			}  });
```

#### Log out 
After having instantiated the widget, calling logout on it will allow you to kill the current session.

```

authorization.logout();

```

#### Example

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





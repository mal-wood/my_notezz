**Include the following in the user model:**
```
#require bcrypt at top of file 

require 'bcrypt'

class User < ActiveRecord::Base 
	validates :first_name, :last_name, :email, presence: true
	validates :email, uniqueness: true
  validates :password_hash, presence: true
   
  include BCrypt

  def password 
    @password ||= Password.new(password_hash)
  end

  def password=(new_password)
    @password = Password.create(new_password)
    self.password_hash = @password
  end

  def self.authenticate(email, new_password)
     user = User.find_by(email: email)
     if user && user.password == new_password
       return user
     else
       nil
     end
   end
end 
```

**Registration**
```
#BASIC REGISTRATION FORM TO CREATE AN ACCOUNT 
<h1> Create Account </h1>
	<form action="/users" method="post">
		<p><input type="text" name="user[first_name]" placeholder="First Name"></p>
		<p><input type="text" name="user[last_name]" placeholder="Last Name"></p>
		<p><input type="text" name="user[email]" placeholder="Email"></p>
		<p><input type="password" name="user[password]" placeholder="Password"></p>
		<p><input type="submit" value="Register"></p>
	</form>
	
#BASIC USER CONTROLLER ROUTES to display login page 
get '/users/new' do 
	erb :'users/new'
end

#BASIC USER ROUTE to save the USER to the database
post '/users' do 
	@user = User.new(params[:user])
	if @user.save 
		redirect "/users/#{@user.id}"
	else 
		erb :'users/new'
	end
end 

get '/users/:id' do  
	@user = User.find(params[:id])
	erb :'users/show'
end


```

**Login** 
```
#SESSION LOGIN 
<h1> Login </h1> 
	<form action='/sessions' method= "post">
		<p><input type="text" name="email" placeholder="Email"></p>
		<p><input type="password" name="password" placeholder="Password"></p>
		<p><input type="submit" value="Login"></p>
	</form>

#SESSION CONTROLLERS 
get '/inspector' do
  session.inspect
end

get '/sessions/new' do 
	erb :'sessions/new'
end

post '/sessions' do 
 	@user = User.authenticate(params[:email], params[:password])
 			if @user 
 				session[:id] = @user.id
 				redirect "/users/#{@user.id}"
 			else
 			erb :'users/new'
 		end
 end
```

**Logout** 

*Include the following in the user model: *
```
#require bcrypt at top of file 

  include BCrypt

  def password 
    @password ||= Password.new(password_hash)
  end

  def password=(new_password)
    @password = Password.create(new_password)
    self.password_hash = @password
  end

  def self.authenticate(email, password)
     user = User.find_by(email: email)
     if user.password == password
       true
     else
       false
     end
   end
```

**Registration**
```
#in USERS view 
<h1> Create Account </h1>
	<form action="/users" method="post">
		<p><input type="text" name="user[first_name]" placeholder="First Name"></p>
		<p><input type="text" name="user[last_name]" placeholder="Last Name"></p>
		<p><input type="text" name="user[email]" placeholder="Email"></p>
		<p><input type="password" name="user[password]" placeholder="Password"></p>
		<p><input type="submit" value="Register"></p>
	</form>
	
#in USERS controller
post '/users' do 
	@user = User.new(params[:user])
	if @user.save 
		redirect "/users/#{@user.id}"
	else 
		erb :'users/new'
	end
end 
```

**Login** 

**Logout** 

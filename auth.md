Include the following in the user model: 
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

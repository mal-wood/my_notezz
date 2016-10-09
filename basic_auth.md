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
#BASIC USER ROUTE to display login page 
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

**Logout** 

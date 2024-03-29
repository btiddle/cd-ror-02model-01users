|     Project | Information                                                  |
|------------:|:-------------------------------------------------------------|
| Domain:     | [Coding Dojo](http://codingdojo.com) (cd)                    |                  
| Topic:      | Ruby on Rails (ror), Understanding Rails - Models (02model)  |
| Assignment: | Users (01users)                                              |
| Repository: | cd-ror-02model-01users                                       |

~~~

rails new user_login_project
cd user_login_project/

edit> Gemfile
  gem 'bcrypt', '~> 3.1.7'    # Using bcrypt (3.1.7)
  gem 'foundation-rails'      # Using foundation-rails (5.4.0.0)
  gem 'hirb'                  # Using hirb (0.7.2)
bundle install

rails generate model User first_name:string last_name:string email_address:string age:integer

edit> User.rb
class User < ActiveRecord::Base
    validates :first_name, :last_name, :email_address, :age, presence: true
    validates :age, numericality: {
        :only_integer => true, 
        :greater_than_or_equal_to => 10,
        :less_than => 150
    }
    validates :first_name, :last_name, :length => { :minimum => 2 }
end

rake db:create
rake db:migrate

rail c
Hirb.enable()

User.create(:first_name => "Amber", :last_name => "Baker", :email_address => "amber@amber.com", :age => 21)
User.create(first_name:"Carmen", last_name:"Dickerson", email_address:"carmen@carmen.com", age:22)
User.create(:first_name => "Dian", :last_name => "Edwards", :email_address => "amber@amber.com", :age => 23)
User.create(first_name:"Fannie", last_name:"Garison", email_address:"fannie@fannie.com", age:24)

user1 = User.new(first_name:"Helen", last_name:"Ison", email_address:"helen@helen.com")
user1.valid?                    # => false
user1.errors
user1.errors.full_messages
user1.save                      # => false, rollback transaction
user1.update(age:66)            # => true, commit transaction (changed age to 66 and committed to db)
user1.valid?                    # => true
User.where(first_name:"Helen")  # id = 5
User.find(5).update(age:25)     # => true, commit transaction

user2 = User.new(first_name:"Helen", last_name:"Ison", email_address:"helen@helen.com", age:26)
user2.valid?                    # true
user2.new_record?               # true
user2.save                      # => true, commit transaction 
User.find(6).update(first_name:"Jane", last_name:"Karson", email_address:"jane@jane.com")

user3 = User.new(first_name:"Linda", last_name:"Madison")
user3.valid?                    # false
user3 = User.new(first_name:"Linda", last_name:"Madison", email_address:"Linda@Linda.com", age:27)
user3.valid?                    # true
user3.new_record?               # true
user3.save                      # => true, commit transaction 
user3.new_record?               # false

user4 = User.new
user4.valid?
user4.errors
user4.errors.full_messages
user4.update(age: 5)
user4.errors.full_messages

User.all
User.first
User.last

User.order("first_name DESC") 
or 
User.order("first_name desc") 
or
User.order(first_name: :desc) 
but not 
User.order(first_name: :DESC)

User.find(3)
User.find(3).update(last_name: "Smith")
User.all

User.find(4)
User.destroy(4)
User.all

~~~

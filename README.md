# Go-Scholar Final Project

## Rules

- Please try to make use of all knowledge that you have gained throughout this course
- This is an individual project, please do this project by yourself
- All code must be committed to one private repository on bitbucket
- Exercise TDD
- You **must** create more than one separate service. Please create justification by yourself on how you design the multiple services (you are allowed to make assumptions)
- Implement at least one type of sync communication and one type of async communication for communicating between services
- Make sure that you understand and can justify your actions for the design and implementation of your application

## Time limit

- Make sure that the last commit and push is before December 20th, 2018 23:59

## Bonus

- One of the service must be in Go
- Implement design patterns that is not built-in on your choice of framework (provide justification)

## Use-case that must be implemented
```
1 User
  * 1.1 User can register
  * 1.2 User can login/logout
  * 1.3 User can see their own profile
    * 1.3.1 User can see their go-pay balance
    * 1.3.2 User can top-up their go-pay
  * 1.4 User can edit their own profile
  * 1.5 User can order go-ride
  * 1.6 User can order go-car
  * 1.7 User can pick payment type during confirmation (cash or go-pay)
  * 1.8 User can see order history

2 Driver
  * 2.1 Driver can register
  * 2.2 Driver can login/logout
  * 2.3 Driver can see their own profile
    * 2.3.1 Driver can see their go-pay balance
  * 2.4 Driver can set their current location (to simulate GPS)
  * 2.5 Driver can bid for job
  * 2.6 Driver can see job history
```

## Note
```
- You are only allowed to ask questions related to the rules, bonus & use-case of this final project.
- Implement & store coordinate for real-world location using google map API or similar services.
- For use-case 2.5, there are two possible approaches that you can take:
  1. Pick the driver automatically based on their current location. 
     Implement an algorithm that distribute the order evenly between drivers 
     (don't let one driver get all the job).
  2. When someone orders go-ride or go-car, notify drivers who are currently located on said location. 
     Driver who accept the order first will get the job. Implementation of the notification system 
     is left to you. (BONUS)
```

---

## Developer's note

This section will describe what i'm doing during developing this project. I will put the spesification and the major schema here.

### Installation Guide

- clone every app inside go-ride repo
- bundle install
- instal postgres sql
- create database goride
- rails db:migrate
- create database allocation
- rails db:migrate
- install apache kafka
- start apache kafka
- start application
- start racecar
- register as user if you are user
- login user and create order
- register as driver if you are driver
- login driver and do job

### Timelines
```
1  creating rail apps
2  designing db schema
3  making unit test for user and driver
4  scaffolding user and driver
5  creating login page for user and driver
6  creating authorization access
7  creating order
8  all transaction process done
9  user cannot cancel
10 calculate nearby driver not implemented
11 working on microservices design
12 create algorithm to select driver near to order origin
13 create other rails app prepare to move allocation service
   - api get index
   - consumer to set driver_location
   - consumer to unset driver_location
   - consumer to set order_id when request comes
   - consumer to produce driver_id
14 create consumer on application
   - consumer can receive driver_id from allocation
   - when complete,
   - produce to change allocation status to online
15 refactoring everything
16 creating view with bootstrap
17 refactoring code
18 refactoring unit test
19 add new feature, driver will move after complete order
```

### Application Presentation
*Application Service*
```
User login & Sign Up
Driver login & Sign Up
Driver set location
User create order
Driver got order
Driver complete / cancel

Scenario :
user order with cash
user order with gopay
user order without driver
driver accept order
driver cancel order
```
*Allocation Service*
```
receive a driver request
search nearby driver to pick up point
giving driver randomly 
```
*Routes Cache Service*
```
save a new added routes
provide an api with recent routes data
```

### Version Spesification

*Monolitic V1*
```
unstable version
order confirm form using post method
driver given during confirmation
rando driver will be given
driver can see their job if order exist
no gopay method implemented
```

*Monolitic V2*
```
implementing gopay method for payment
order confirm form using get method
driver given during confirmation
lat lng available order confirm
```

*Monolitic V3*
```
refactoring test that is necessary
refactoring model and controller
```

*Monolitic V4*
```
unstable
full features monolitic application
completed all use case in monolitic manners
async driver allocation
user can wait for their driver until arived
```

*Monolitic V5*
```
stable version monolitic
full features on use case
using async when allocating driver
```

*Microservices V05*
```
stable async comunication between
application <--> allocation
allocating driver asyncly
```

### DB schema

 DB version : PostgreSQL 9.6

*Users*
```
  id
  username
  password
  full_name
  email
  phone
  address
  credit
```

*Drivers*
```
  id
  username
  password
  full_name
  email
  phone
  address
  type
  credit
```

*DriverLocations*
```
  id
  driver_id
  service_type
  location
  lat
  lng
  status [offline, online, busy]
  order_id
```

*Orders*
```
  id
  user_id
  driver_id
  origin
  destination
  distance
  service_type
  payment_type
  price
  status  [complete, canceled, on_progress]
```


---

## Notes

Somethings that maybe important..

### External usefull links

[For requesting API](https://github.com/c42/wrest)

[For HTTP request](https://github.com/httprb/http)

[Google map API for location lat lng](https://maps.googleapis.com/maps/api/geocode/json?latlng=40.714224,-73.961452&key=AIzaSyD9eO9WPUr-KKTqUM8Q3uzHcZpThY4NIDM)

[Google map Distance API with lat lng](https://maps.googleapis.com/maps/api/distancematrix/json?units=imperial&origins=40.714224,-73.961452&destinations=40.714224,-73.961455&key=AIzaSyD9eO9WPUr-KKTqUM8Q3uzHcZpThY4NIDM)

### Setting up postgres

```
HOW TO SETUP POSTGRES ON RAILS

sudo apt-get install libpq-dev
gem install pg
install pgadmin untuk GUI

gem 'pg'

database.yml
default: &default
  adapter: postgresql
  encoding: unicode
  pool: 5
  host: localhost
  username: postgres
  password: umarkotak

development:
  <<: *default
  database: goride

test:
  <<: *default
  database: goride
```

### Setting Up Apache Kafka

[Installation guide](https://devops.profitbricks.com/tutorials/install-and-configure-apache-kafka-on-ubuntu-1604-1/)

[Example files](https://devops.profitbricks.com/tutorials/install-and-configure-apache-kafka-on-ubuntu-1604-1/)

[Apache kafka files](https://www.apache.org/dyn/closer.cgi?path=/kafka/1.0.0/kafka_2.11-1.0.0.tgz)

```
1. Install java
2. Install zookeeper
3. Install apache kafka
   - download the file
   - sudo tar -xvf kafka_versions.tgz -C /usr/local/kafka/
   - start kafka
   - sudo /usr/local/kafka/kafka_2.11-1.0.0/bin/kafka-server-start.sh /usr/local/kafka/kafka_2.11-1.0.0/config/server.properties
4. gem install ruby-kafka
5. git clone https://github.com/rodolfobandeira/ruby-kafka.git
6. open the producer.rb and consumer.rb
7. sudo nohup /usr/local/kafka/kafka_2.11-1.0.0/bin/kafka-server-start.sh /usr/local/kafka/kafka_2.11-1.0.0/config/server.properties
8. start consumer app
9. execute producer, consumer will take what producer give
```

### Setting Up racecar

[racecar](https://github.com/zendesk/racecar)

```
1. gem 'racecar'
2. bundle install
3. bundle exec rails generate racecar:install
4. rails generate racecar:consumer TapDance
5. bundle exec racecar --require dance_moves TapDanceConsumer
6. racecar ConsumerTestConsumer -> to start consumer
```

### Usefull command

```
1. puma -C config/puma.rb -d -p 3001 -> to start rails s in back ground
2. kill -s 9 id -> kill service by id
```

### Junkyard

```

```

### How To Start
1. Start apache kafka
```
sudo /usr/local/kafka/kafka_2.11-1.0.0/bin/kafka-server-start.sh /usr/local/kafka/kafka_2.11-1.0.0/config/server.properties
```

2. Start application service
```
cd ~/goscholar/go-ride/application_service
puma -C config/puma.rb -d -p 3000
racecar ApplicationConsumer
```

3. Start allocation service
```
cd ~/goscholar/go-ride/allocation_service
puma -C config/puma.rb -d -p 3001
racecar AllocationConsumer
```

4. Start routes cache services
```
cd ~/goscholar/go-ride/routes_cache_service
./routes_cache_service
```

5. Userlist
```
useradmin; useradmin
```

6. Driverlist
```
driveradmin; driveradmin
```

7. To stop all
```
ps aux | grep puma
```